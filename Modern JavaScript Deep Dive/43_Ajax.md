# **43장 Ajax**

- [**43장 Ajax**](#43장-ajax)
  - [**들어가며 🎈**](#들어가며-)
  - [**Ajax란?**](#ajax란)
  - [**JSON**](#json)
    - [**JSON 표기 방식**](#json-표기-방식)
    - [**JSON.stringify**](#jsonstringify)
    - [**JSON.parse**](#jsonparse)
  - [**XMLHttpRequest**](#xmlhttprequest)
    - [**XMLHttpRequest 객체 생성**](#xmlhttprequest-객체-생성)
    - [**XMLHttpRequest 객체의 프로퍼티와 메서드**](#xmlhttprequest-객체의-프로퍼티와-메서드)
    - [**HTTP 요청 전송**](#http-요청-전송)
    - [**HTTP 응답 처리**](#http-응답-처리)

<br>

## **들어가며 🎈**

본 정리글은 이웅모강사님의 모던 자바스크립트 Deep Dive를 공부하고 습득하기 위해 제 나름대로 정리한 글 입니다. 보다 상세한 정보를 원하신다면 해당 서적을 참고하시길 바랍니다.

<br>

## **Ajax란?**

Ajax(_Asynchronous JavaScript and XML_)란?

자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.

Ajax는 브라우저에서 제공하는 Web API인 `XMLHttpRequest` 객체를 기반으로 동작한다.

이전의 웹페이지는 `html` 태그로 시작해서 `html` 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다.

- 전통적 방식의 단점
  - 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신이 발생한다.
  - 변경할 필요가 없는 부분까지 처음부터 다시 렌더링한다. 이로 인해 화면 전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생한다.
  - 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹된다.

서버로부터 웹페이지의 변경에 필요한 데이터만 비동기 방식으로 전송받아 웹페이지를 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 필요가 있는 부분만 한정적으로 렌더링하는 방식이 가능해진 것이다. 이를 통해 브라우저에서도 데스크톱 애플리케이션과 유사한 빠른 퍼포먼스와 부드러운 화면 전환이 가능해졌다.

- Ajax의 장점
  - 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않는다.
  - 변경할 필요가 없는 부분은 다시 렌더링하지 않는다. 따라서 화면이 순간적으로 깜빡이는 현상이 발생하지 않는다.
  - 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

<br>

---

## **JSON**

JSON(JavaScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.

자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용할 수 있다.

<br>

### **JSON 표기 방식**

- JSON은 자바스크립트이 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다.

```json
{
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```

- JSON의 키는 반드시 큰따옴표(작은따옴표 사용 불가)로 묶어야 한다.
- 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다.
- 하지만 문자열은 반드시 큰따옴표로 묶어야 한다.

<br>

### **JSON.stringify**

- `JSON.stringify` 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화(_serializing_)라 한다.

```javascript
const obj = {
  name: 'Lee',
  age: 20,
  alive: true,
  hobby: ['traveling', 'tennis'],
};

// 객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
console.log(typeof json, json);
// string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}

// 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
const prettyJson = JSON.stringify(obj, null, 2);
console.log(typeof prettyJson, prettyJson);
/* 
string {
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/

// replacer 함수. 값의 타입이 Number이면 필터링되어 반환하지 않는다.
function filter(key, value) {
  // undefined: 반환하지 않음
  return typeof value === 'number' ? undefined : value;
}

// JSON.stringify 메서드에 두 번째 인수로 replacer 함수를 전달한다.
const strFilteredObject = JSON.stringify(obj, filter, 2);
console.log(strFilteredObject);
/* 
{
  "name": "Lee",
  "alive": true,
  "hobby": [
    "traveling",
    "tennis"
  ]
}
*/
```

- `JSON.stringify` 메서드는 객체뿐만 아니라 배열도 JSON 포맷의 문자열로 변환한다.

```javascript
const todos = [
  { id: 3, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 1, content: 'Javascript', completed: false },
];

const json = JSON.stringify(todos, null, 2);
console.log(typeof json, json);

/*
string[
  {
    id: 3,
    content: 'HTML',
    completed: false,
  },
  {
    id: 2,
    content: 'CSS',
    completed: true,
  },
  {
    id: 1,
    content: 'Javascript',
    completed: false,
  }
];
*/
```

<br>

### **JSON.parse**

- `JSON.parse` 메서드는 JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다.
- 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 역직렬화(_deserializing_)라 한다.

```javascript
const parsed = JSON.parse(json);
console.log(typeof parsed, parsed);
// object { name: 'Lee', age: 20, alive: true, hobby: [ 'traveling', 'tennis' ] }
```

- 배열이 JSON 포맷의 문자열로 변환되어 있는 경우 `JSON.parse`는 문자열을 배열 객체로 변환한다. 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

<br>

---

## **XMLHttpRequest**

### **XMLHttpRequest 객체 생성**

### **XMLHttpRequest 객체의 프로퍼티와 메서드**

**XMLHttpRequest 객체의 프로토타입 프로퍼티**
**XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티**
**XMLHttpRequest 객체의 메서드**
**XMLHttpRequest 객체의 정적 프로퍼티**

### **HTTP 요청 전송**

**XMLHttpRequest.prototype.open**
**XMLHttpRequest.prototype.send**
**XMLHttpRequest.prototype.setRequestHeader**

### **HTTP 응답 처리**
