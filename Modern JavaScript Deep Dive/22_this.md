# **21 this**

<br>

## **들어가며 🎈**

본 정리글은 이웅모강사님의 모던 자바스크립트 Deep Dive를 공부하고 습득하기 위해 제 나름대로 정리한 글 입니다. 보다 상세한 정보를 원하신다면 해당 서적을 참고하시길 바랍니다.

<br>

## **this 키워드**

동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야한다 이때 메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다. 이를 위해 자바스크립트는 `this`라는 특수한 식별자를 제공한다.

`this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수(_self-referencing variable_)다. `this`를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

`this`는 자바스크립트 엔진에 의해 암묵적으로 생성되며, 코드 어디서든 참조할 수 있다. 함수를 호출하면 `argument` 객체와 `this`가 암묵적으로 함수 내부에 전달된다. 함수 내부에서 `arguments` 객체를 지역 변수처럼 사용할 수 있는 것처럼 `this`도 지역 변수처럼 사용할 수 있다. 단, `this`가 가리키는 값, 즉 `this` 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

> 📄 **this 바인딩**
>
> 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어, 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. `this` 바인딩은 `this`(키워드로 분류되지만 식별자 역할)와 `this`가 가리킬 객체를 바인딩하는 것이다.

타 클래스 기반 언어에서 `this`는 언제나 클래스가 생성하는 인스턴스를 가리킨다. 하지만 자바스크립트의 `this`는 함수가 호출되는 방식에 따라 `this`에 바인딩될 값, 즉 `this` 바인딩이 동적으로 결정된다. 또한 strict mode 역시 `this` 바인딩에 영향을 준다.

`this`는 코드 어디에서든 참조 가능하지만, 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다. 따라서 strict mode가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩된다. 일반 함수 내부에서 `this`를 사용할 필요가 없기 때문이다.

<br>

---

## **함수 호출 방식과 this 바인딩**

> 📄 렉시컬 스코프와 `this` 바인딩은 결정 시기가 다르다
>
> 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 `this` 바인딩은 함수 호출 시점에 결정된다.

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출

<br>

### **일반 함수 호출**
기본적으로 `this`에는 전역 객체가 바인딩된다.

```javascript
function foo() {
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

전역 함수는 물론이고 중첩 함수를 일반 함수로 호출하면 함수 내부의 `this`에는 전역 객체가 바인딩된다. strict mode 가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩된다.

```javascript
function foo() {
  'use strict';

  console.log("foo's this: ", this); // undefined
  function bar() {
    console.log("bar's this: ", this); // undefined
  }
  bar();
}
foo();
```

메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 `this`에는 전역 객체가 바인딩된다.

```javascript
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value = 1;
// const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.
// const value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: f}
    console.log("foo's this.value: ", this.value); // 100

    function bar() {
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }

    // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는
    // 전역 객체가 바인딩된다.
    bar();
  },
};

obj.foo();
```
콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 `this`에도 전역 객체가 바인딩된다. 어떠한 함수라도 일반 함수로 호출되면 `this`에 전역 객체가 바인딩된다.

```javascript
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: f}
    console.log("foo's this.value: ", this.value); // 100
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  },
};

obj.foo();
```

이처럼 일반 함수로 호출된 모든 함수(중첩, 콜백 함수 포함) 내부에 `this`에는 전역 객체가 바인딩된다. 외부함수의 메서드와 중첩이나 콜백 함수가 `this`가 일치하지 않는다는 것은 헬퍼 함수로서 동작하기 어렵게 만든다.

메서드 내부의 중첩함수나 콜백함수의 `this` 바인딩을 메서드의 `this`와 일치시키기 위한 방법은 다음과 같다.

```javascript
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)를 변수 that에 할당
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조
    setTimeout(function () {
      console.log(that.value);
    }, 100);
  },
};

obj.foo();
```
위 방법 이외에도 자바스크립트는 `this`를 명시적으로 바인딩할 수 있는 메서드를 제공한다.
- `Function.prototype.apply`
- `Function.prototype.call`
- `Function.prototype.bind`

또는 화살표 함수를 사용해서 `this` 바인딩을 일치시킬 수도 있다.

<br>

### **메서드 호출**

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(`.`) 연산자 앞에 기술한 객체가 바인딩된다. 주의할 것은 메서드 내부의 `this`는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는것이다.

```javascript
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다
    return this.name;
  },
};

// 메서드 getName을 호출한 개게는 person이다
console.log(person.getName()); // Lee

const anotherPerson = {
  name: 'Kim',
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
```

메서드 내부의 `this`는 프로퍼티 메서드를 가리키고 있는 객체와는 관계가 없고 메서드를 호출한 객체에 바인딩된다. 이는 프로토타입 메서드 내부에서 사용된 `this`도 마찬가지.

<br>

### **생성자 함수 호출**
생성자 함수 내부의 `this`에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

<br>

### **Function.prototype.apply/call/bind 메서드에 의한 간접 호출**
`apply`, `call` 메서드는 `this`로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다. `apply`와 `call` 메서드의 사용법은 다음과 같다.

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

<br>

---

✔ **정리**

<center>함수 호출 방식</center> | <center>this 바인딩</center>
--- | --- 
일반 함수 호출 | 전역 객체
메서드 호출 | 메서드를 호출한 객체
생성자 함수 호출 | 생성자 함수가 (미래에) 생성할 인스턴스
`Function.prototype.apply/call/bind` 메서드에 의한 간접 호출 | `Function.prototype.apply/call/bind` 메서드애 첫번째 인수로 전달한 객체