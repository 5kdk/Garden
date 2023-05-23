# **33장 7번째 데이터 타입 Symbol**

- [**33장 7번째 데이터 타입 Symbol**](#33장-7번째-데이터-타입-symbol)
  - [**들어가며 🎈**](#들어가며-)
  - [**심벌이란?**](#심벌이란)
  - [**심벌 값의 생성**](#심벌-값의-생성)
    - [**Symbol 함수**](#symbol-함수)
    - [**Symbol.for/Symbol.keyFor 메서드**](#symbolforsymbolkeyfor-메서드)
  - [**심벌과 상수**](#심벌과-상수)
  - [**심벌과 프로퍼티 키**](#심벌과-프로퍼티-키)
  - [**심벌과 프로퍼티 은닉**](#심벌과-프로퍼티-은닉)
  - [**심벌과 표준 빌트인 객체 확장**](#심벌과-표준-빌트인-객체-확장)
  - [**Well-known Symbol**](#well-known-symbol)

<br>

## **들어가며 🎈**

본 정리글은 이웅모강사님의 모던 자바스크립트 Deep Dive를 공부하고 습득하기 위해 제 나름대로 정리한 글 입니다. 보다 상세한 정보를 원하신다면 해당 서적을 참고하시길 바랍니다.

<br>

## **심벌이란?**

심벌은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다. 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 따라서 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

<br>

---

## **심벌 값의 생성**

### **Symbol 함수**

- 심벌 값은 `Symbol` 함수를 호출하여 생성한다.
- 다른 원시값은 리터럴 표기법을 통해 값을 생성할 수 있지만 심벌 값은 `Symbol` 함수를 호출하여 생성해야 한다.
- 생성된 심벌 값은 외부로 노출되지 않아 확인 할 수 없다.
- 다른 값과 절대 중복되지 않는 유일무이한 값이다.

```javascript
const mySymbol = Symbol();

console.log(typeof mySymbol); // symbol

console.log(mySymbol); // Symbol()
```

- 다른 생성자 함수와는 달리 `new` 연산자와 함께 호출하지 않는다.
- `new` 연산자와 함께 생성자 함수 또는 클래스를 호출하면 객체(인스턴스)가 생성되지만 심벌 값은 변경 불가능한 원시 값이다.
  ```javascript
  new Symbol(); // TypeError: Symbol is not a constructor
  ```
- 선택적으로 문자열을 인수로 전달할 수 있다.

  - 생성된 심벌 값에 대한 설명으로 디버깅 용도로 사용된다.
  - 심벌 값 생성에 어떠한 영향도 주지 않는다.
  - 즉, 설명이 같더라도 생성된 심벌은 유일무이한 값이다.

    ```javascript
    const mySymbol1 = Symbol('mySymbol');
    const mySymbol2 = Symbol('mySymbol');

    console.log(mySymbol1 === mySymbol2); // false
    ```

- 심벌 값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.
- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
- 단, 불리언 타입으로는 암묵적으로 타입 변환한다.

<br>

### **Symbol.for/Symbol.keyFor 메서드**

`Symbol.for` 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서(_global symbol registry_)에서 해당 키와 일치하는 심벌값을 검색한다.

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
- 검색에 실패하면 새로운 심벌 값을 생성하여 `Symbol.for` 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

  ```javascript
  // 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
  const s1 = Symbol.for('mySymbol');
  // 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
  const s2 = Symbol.for('mySymbol');

  console.log(s1 === s2); // true
  ```

- 자바스크립트 엔진이 관리하는 심벌 값 저장소인 전역 심벌 레지스트리에서 심벌 값을 검색할 수 있는 키를 지정할 수 없으므로 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
- `Symbol.for` 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 유일무이하 상수인 심벌 값을 단하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.

`Symbol.keyFor` 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```javascript
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
console.log(Symbol.keyFor(s1)); // -> mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
console.log(Symbol.keyFor(s2)); // -> undefined
```

<br>

---

## **심벌과 상수**

```javascript
/* const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4
};
 */

const Direction = {
  UP: Symbol('up'),
  DOWN: Symbol('down'),
  LEFT: Symbol('left'),
  RIGHT: Symbol('right'),
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log('You are going UP.');
}
```

- 위 예제는 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우, 이때 `1`, `2`, `3`, `4`가 변경될수 있으며, 다른 변수 값과 중복될 수도 있다.
- 이런 경우 변경/중복될 가능성이 있는 무의미한 상수 대신 유일무이한 심벌 값을 사용할 수 있다.

<br>

---

## **심벌과 프로퍼티 키**

```javascript
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for('mySymbol')]: 1,
};
```

심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.(기존과 미래 모두 충돌 위험이 없다.)

<br>

---

## **심벌과 프로퍼티 은닉**

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 `for ... in` 문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로 찾을 수 없다.

- 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.
- 완벽하게 숨길 수는 없은 것이 ES6에서 도입된 `Object.getOwnPropertySymbols` 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

<br>

---

## **심벌과 표준 빌트인 객체 확장**

- 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.
  - 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.
  - 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문
- 중복 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 안전하게 표준 빌트인 객체를 확장할 수 있다.

<br>

---

## **Well-known Symbol**

- 자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다. 빌트인 심벌 값은 `Symbol` 함수의 프로퍼티에 할당되어 있다.
  - 이를 ECMAScript 사양에서는 Well-Known Symbol 이라 부른다.
- 예를 들어 `Array`, `String`, `Map`, `Set`, `TypedArray`, `arguments`, `NodeList`, `HTMLCollection`과 같이 `for ... of` 문으로 순회 가능한 빌트인 이터러블은 Well-known Symbol인 `Symbol.iterator`를 키로 갖는 메서드를 가지며, `Symbol.iterator` 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.
- 빌트인 이터러블은 이 규정 즉, 이터레이션 프로토콜을 준수한다.
- 만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면
  - `Symbol.iterator`를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 된다.
