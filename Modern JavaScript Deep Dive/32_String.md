# **32장 String**

- [**32장 String**](#32장-string)
  - [**들어가며 🎈**](#들어가며-)
  - [**String 생성자 함수**](#string-생성자-함수)
  - [**length 프로퍼티**](#length-프로퍼티)
  - [**String 메서드**](#string-메서드)
    - [**String.prototype.indexOf**](#stringprototypeindexof)
    - [**String.prototype.search**](#stringprototypesearch)
    - [**String.prototype.includes**](#stringprototypeincludes)
    - [**String.prototype.startsWith**](#stringprototypestartswith)
    - [**String.prototype.endsWith**](#stringprototypeendswith)
    - [**String.prototype.charAt**](#stringprototypecharat)
    - [**String.prototype.substring**](#stringprototypesubstring)
    - [**String.prototype.slice**](#stringprototypeslice)
    - [**String.prototype.toUpperCase**](#stringprototypetouppercase)
    - [**String.prototype.toLowerCase**](#stringprototypetolowercase)
    - [**String.prototype.trim**](#stringprototypetrim)
    - [**String.protype.repeat**](#stringprotyperepeat)
    - [**String.prototype.replace**](#stringprototypereplace)
    - [**String.prototype.split**](#stringprototypesplit)

<br>

## **들어가며 🎈**

본 정리글은 이웅모강사님의 모던 자바스크립트 Deep Dive를 공부하고 습득하기 위해 제 나름대로 정리한 글 입니다. 보다 상세한 정보를 원하신다면 해당 서적을 참고하시길 바랍니다.

표준 빌트인 객체인 `String`은 원시 타입인 문자열을 다룰 때 유용한 프로퍼티와 메서드를 제공한다.

<br>

## **String 생성자 함수**

- `new` 연산자와 함께 호출하여 `String` 인스턴스를 생성할 수 있다.
- `String` 생성자 함수에 인수를 전달하지 않고 `new` 연산자와 함께 호출하면 `[[StringData]]` 내부 슬롯에 빈 문자열을 할당한 `String` 래퍼 객체를 생성한다.
  - ES5에서는 `[[StringData]]`를 `[[PrimitiveValue]]`라 불렀다.
- `String` 생성자 함수의 인수로 문자열을 전달하면서 `new` 연산자와 함께 호출하면 `[[StringData]]` 내부 슬롯에 인수로 전달받은 문자열을 할당한 `String` 래퍼 객체를 생성한다.
  - `String` 레퍼 객체는 배열과 마찬가지로 `length` 프로파티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다.
  - 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.
  - 문자열은 원시 값이므로 변경할 수 없다. 이때 에러가 발생하지 않는다.
- `String` 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후 `[[StringData]]` 내부 슬롯에 변환된 문자열을 할당한 `String` 래퍼 객체를 생성한다.
- `new` 연산자를 사용하지 않고 `String` 생성자 함수를 호출하면 `String` 인스턴스가 아닌 문자열을 반환한다.
  - 이를 이용하여 명시적으로 타입을 변환하기도 한다.

<br>

---

## **length 프로퍼티**

- `String` 래퍼 객체는 배열과 마찬가지로 `length` 프로퍼티를 갖는다.
- 인덱스를 나타내는 숫자를 프로퍼티 키로, 각 문자를 프로퍼티 값으로 가지므로 `String` 래퍼 객체는 유사 배열 객체다.

<br>

---

## **String 메서드**

- `String` 객체에는 원본 `String` 래퍼 객체를 직접 변경하는 메서드는 존재하지 않는다.
- 즉, `String` 객체의 메서드는 언제나 새로운 문자열을 반환한다.
- 문자열은 변경 불가능(_imumutable_)한 원시 값이기 때문에 `String` 래퍼 객체도 읽기 전용(_read only_) 객체로 제공된다.

```javascript
const strObj = new String('Lee');

console.log(Object.getOwnPropertyDescriptors(strObj));

/* 
{
  '0': {
    value: 'L',
    writable: false,
    enumerable: true,
    configurable: false
  },
  '1': {
    value: 'e',
    writable: false,
    enumerable: true,
    configurable: false
  },
  '2': {
    value: 'e',
    writable: false,
    enumerable: true,
    configurable: false
  },
  length: { value: 3, writable: false, enumerable: false, configurable: false }
}
*/
```

<br>

### **String.prototype.indexOf**

- 대상 문자열(메서드를 호출한 문자열)에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다. 검색에 실패하면 `-1`를 반환한다.

<br>

### **String.prototype.search**

- 대상 문자열에서 인수로 전달받은 정규 표현식고 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다. 검색에 실패하면 `-1`를 반환한다.

```javascript
const str = 'Hello world';

str.search(/o/); // -> 4
str.search(/x/); // -> -1
```

<br>

### **String.prototype.includes**

- ES6에서 도입, 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 `true` `false` 로 반환한다.
- 메서드의 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

<br>

### **String.prototype.startsWith**

- ES6에서 도입, 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 `true` `false` 로 반환한다.
- 메서드의 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

<br>

### **String.prototype.endsWith**

- ES6에서 도입, 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 `true` `false` 로 반환한다.
- 메서드의 두번째 인수로 검색할 문자열의 길이를 전달할 수 있다.

<br>

### **String.prototype.charAt**

- 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.
- 인덱스는 문자열의 범위, 즉 0 ~ (문자열의 길이 - 1) 사이의 정수이어야 한다. 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환한다.
- `chatAt` 메서드와 유사한 문자열 메서드는 `String.prototype.charCodeAt`과 `String.prototype.codePointAt`이 있다.

<br>

### **String.prototype.substring**

- 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.

  ```javascript
  const str = 'Hello World';

  // 인데스 1부터 인덱스 4 이전까지의 부분 문자열을 반환한다.
  str.substring(1, 4); // -> ell
  ```

  <br>

- `substring` 메서드의 두 번째 인수는 생략할 수 있다. 이때 첫 번째 인수로 전달한 인덱스에 위치하는 문자부터 마지막 문자까지 부분 문자열을 반환한다.

  ```javascript
  const str = 'Hello World';

  // 인덱스 1부터 마지막 문자까지 부분 문자열을 반환한다.
  str.substring(1); // -> 'ello World'
  ```

  <br>

- `substring` 메서드의 첫 번째 인수는 두 번째 인수보다 작은 정수이어야 정상이다. 하지만 다음과 같이 인수를 전달하여도 정상 동작한다.

  - `첫 번째 인수 > 두 번째 인수`인 경우 두 인수는 교환된다.
  - `인수 < 0` 또는 `NaN`인 경우 `0`으로 취급된다
  - `인수 > 문자열의 길이(str.length)`인 경우 인수는 문자열의 길이(`str.length`)로 취급된다.

  ```javascript
  const str = 'Hello World'; // str.length == 11

  // 첫 번째 인수 >  두 번째 인수인 경우 두 인수는 교환된다.
  str.substring(4, 1); // -> 'ell'

  // 인수 < 0 또는 NaN인 경우 0으로 취급된다
  str.substring(-2); // -> 'Hello World'

  // 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)로 취급된다.
  str.substring(1, 100); // -> 'ello World'
  str.substring(20); // -> ''
  ```

<br>

- `String.prototype.indexOf` 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득할 수 있다.

  ```javascript
  const str = 'Hello World';

  // 스페이스를 기준으로 앞에 있는 부분 문자열 취득
  str.substring(0, str.indexOf(' ')); // -> 'Hello'

  // 스페이스를 기준으로 앞에 있는 부분 문자열 취득
  str.substring(0, str.indexOf(' ') + 1, str.length); // -> 'World'
  ```

<br>

### **String.prototype.slice**

- `slice` 메서드는 `substring` 메서드와 동일하게 동작한다.
- 단, `slice` 메서드에는 음수인 인수를 전달할 수 있다.
- 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

<br>

### **String.prototype.toUpperCase**

대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

<br>

### **String.prototype.toLowerCase**

대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

<br>

### **String.prototype.trim**

- 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열은 반환한다.
- `String.protype.trimStart`, `String.protype.trimEnd`를 사용하면 대상 문자열 앞 또는 뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.
- `String.prototype.replace` 메서드에 정규식을 인수로 전달하여 공백 문자를 제거할 수도 있다.

<br>

### **String.protype.repeat**

- ES6에서 도입, 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다.
- 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 `RangeError`를 발생시킨다.
- 인수를 생략하면 기본값 0이 설정된다.

<br>

### **String.prototype.replace**

- 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.
- `replace` 메서드의 두 번째 인수로 치환 함수를 전달할 수 있다.

<br>

### **String.prototype.split**

- 대상 문자열에서 첫 번째 인수로 전달한 문자열 똔ㄴ 정규 표현식을 검색하여 문자열을 구분 한 후 분리된 각 문자열로 이루어진 배열은 반환한다.
- 인수로 빈 문자열은 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
