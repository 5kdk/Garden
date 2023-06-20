# **fill 메서드에 대한 고찰**

### **개요**

- 오랜만에 알고리즘 공부하다 2차원 배열을 다음과 같이 초기화 하려 시도했다.

  ```js
  let result = Array(length).fill([]);
  ```

- 이후 문제 해결 로직을 제대로 구현했으나. 각 2차원 배열에 같은 값이 들어가 있는게 아닌가...?
- "같은 배열 참조값으로 채워졌구나~" 생각하고, `let result = Array.from({ length }, () => []);`와 같이 변경해서 해결했으나, 문득 `fill` 메서드 내부 구현이 궁금해졌다.

<br>

### **발견**

- [ECMA 상세](https://tc39.es/ecma262/multipage/indexed-collections.html#sec-array.prototype.fill)로 살펴본 `fill` 알고리즘 스텝

  > `Array.prototype.fill ( value [ , start [ , end ] ] )`

  1. `ToObject(this value)`: `this` 값을 객체로 변환하여 `O`에 할당한다.
  2. `LengthOfArrayLike(O)`: `O` 객체의 유사 배열 속성(`array-like`)인 `length` 값을 가져와서 `len`에 할당한다.
  3. `ToIntegerOrInfinity(start)`: `start` 값을 정수로 변환하거나 `-Infinity`로 변환한다.
  4. `-∞` 경우, `k` 값을 0으로 설정한다.
  5. `relativeStart`가 0보다 작은 경우, `len + relativeStart` 값과 0 중에서 더 큰 값을 선택하여 `k`에 할당한다.
  6. 그렇지 않으면, `relativeStart` 값을 `len`과 비교하여 더 작은 값을 `k`에 할당한다.
  7. `end` 값이 정의되지 않은 경우, `relativeEnd`를 `len` 값으로 설정한다. 그렇지 않은 경우, `end` 값을 정수로 변환하거나 `-Infinity`로 변환한 값을 `relativeEnd`에 할당한다.
  8. `-∞` 경우, `final` 값을 0으로 설정한다.
  9. `relativeEnd`가 0보다 작은 경우, `len + relativeEnd` 값과 0 중에서 더 큰 값을 선택하여 `final`에 할당한다.
  10. 그렇지 않으면, `relativeEnd` 값을 `len`과 비교하여 더 작은 값을 `final`에 할당한다.
  11. `k`가 `final`보다 작을 동안 반복한다.
      a. `𝔽(k)`를 문자열로 변환하여 `Pk`에 할당한다.
      b. `Set(O, Pk, value, true)`를 수행하여 `O` 객체의 `Pk` 위치에 `value` 값을 설정한다. (`true`는 알고리즘에 따라 해당 위치에 값을 설정하도록 지정하는 옵션이다.)
      c. `k`를 1 증가시킨다.
  12. `O`를 반환한다.

- [MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/fill)의 `fill` 메서드 폴리필을 참고하여 좀 더 쉽게 살펴보자.

  ```js
  if (!Array.prototype.fill) {
    Object.defineProperty(Array.prototype, 'fill', {
      value: function (value) {
        // Steps 1-2.
        if (this == null) {
          throw new TypeError('this is null or not defined');
        }

        var O = Object(this);

        // Steps 3-5.
        var len = O.length >>> 0;

        // Steps 6-7.
        var start = arguments[1];
        var relativeStart = start >> 0;

        // Step 8.
        var k =
          relativeStart < 0
            ? Math.max(len + relativeStart, 0)
            : Math.min(relativeStart, len);

        // Steps 9-10.
        var end = arguments[2];
        var relativeEnd = end === undefined ? len : end >> 0;

        // Step 11.
        var final =
          relativeEnd < 0
            ? Math.max(len + relativeEnd, 0)
            : Math.min(relativeEnd, len);

        // Step 12.
        while (k < final) {
          O[k] = value;
          k++;
        }

        // Step 13.
        return O;
      },
    });
  }
  ```

- step 12를 보면 `O[k]`에 매개변수 `value`를 할당 해주고 있다.
- `value`가 객체라면 참조값이 할당될 것.(배열은 객체이니 참조 값이 할당되었던 것)
- 정리하자면 `Array(length).fill([])`을 사용하여 `result` 배열을 초기화하려고 했지만, 이렇게 초기화하는 방식은 모든 요소가 동일한 빈 배열을 참조하게 된다. `result` 배열의 각 요소가 모두 같은 배열을 가리키게 되기 때문에 생긴 문제였다.

<br>

### **ECMAScript는 왜 fill 메서드를 이런 방식으로 구현해놓았을까?**

> chatGPT 인용

- ECMAScript(JavaScript)에서 `fill` 메서드가 동작하는 방식은 설계 결정에 따라 정해진 것이다. `fill` 메서드는 특정한 값 또는 객체로 배열을 채우는 목적을 가지고 있으며, 배열의 각 요소에 해당 값을 할당한다. 이렇게 설계된 이유는 다음과 같다:

  1. 메모리 효율성: `fill` 메서드는 주어진 값 또는 객체를 배열 요소에 복사하여 할당한다. 만약 `fill` 메서드가 각 요소에 새로운 배열을 할당하도록 동작한다면, 메모리 사용량이 증가하여 효율성이 떨어질 수 있다. 배열 요소가 동일한 배열을 참조하도록 함으로써 메모리 사용을 최적화할 수 있다.

  2. 일관성: `fill` 메서드는 배열의 각 요소를 동일한 값 또는 객체로 채우는 일관된 방법을 제공한다. 이는 코드 작성과 이해에 일관성을 부여하여 프로그램의 가독성과 유지보수성을 향상시킨다. `fill` 메서드를 사용하면 반복문을 통해 각 요소를 개별적으로 설정하는 것보다 코드가 간결해지고 명확해질 수 있다.

  3. 기존 동작과의 일관성: `fill` 메서드의 동작 방식은 기존 배열 메서드 및 전역 객체의 일부 메서드와 일관성을 유지한다. 예를 들어 `Array.from` 메서드로 생성된 배열의 요소에도 `fill` 메서드를 사용할 수 있으며, `Int8Array`, `Uint8Array` 등의 타입 배열도 동일한 방식으로 `fill` 메서드를 사용할 수 있다. 이러한 일관성은 개발자들이 JavaScript의 다양한 기능과 메서드를 익히고 사용하기 쉽도록 도와준다.

- 따라서 `fill` 메서드가 각 배열 요소에 별도의 값을 할당하지 않는 이유는, 메모리 효율성, 일관성, 그리고 기존 동작과의 일관성을 유지하기 위함이다. 개발자는 이를 고려하여 코드를 작성하고, 필요에 따라 배열 요소를 개별적으로 설정하는 방법을 선택할 수 있다.

<br>

### **대체할 문법들 정리해보자**

```js
// 1-1
let result = [];

for (let i = 0; i < length; i++) {
  result.push([]);
}

// 1-2
let result = [];

let count = 0;
for (let i = 0; i < length; i++) {
  result[count++] = [];
}

// 2
let result = Array.from({ length }, () => []);

// 3
let result = Array(length)
  .fill()
  .map(() => []);
```
