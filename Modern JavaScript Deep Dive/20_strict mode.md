# **20 strict mode**

- [**20 strict mode**](#20-strict-mode)
  - [**들어가며 🎈**](#들어가며-)
  - [**strict mode란?**](#strict-mode란)
  - [**strict mode의 적용**](#strict-mode의-적용)
  - [**전역에 strict mode를 적용하는 것은 피하자**](#전역에-strict-mode를-적용하는-것은-피하자)
  - [**함수 단위로 strict mode를 적용하는 것도 피하자**](#함수-단위로-strict-mode를-적용하는-것도-피하자)
  - [**strict mode가 발생시키는 에러**](#strict-mode가-발생시키는-에러)
  - [**strict mode 적용에 의한 변화**](#strict-mode-적용에-의한-변화)
    - [**일반 함수의 this**](#일반-함수의-this)
    - [**arguments 객체**](#arguments-객체)


<br>

## **들어가며 🎈**

본 정리글은 이웅모강사님의 모던 자바스크립트 Deep Dive를 공부하고 습득하기 위해 제 나름대로 정리한 글 입니다. 보다 상세한 정보를 원하신다면 해당 서적을 참고하시길 바랍니다.

<br>

## **strict mode란?**

ES5에 추가된 `strict mode`(엄격 모드)는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

ESLint 같은 린트 도구를 사용해도 유사한 효과를 얻을 수 있는데, 정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류만이 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅해주는 유용한 도구다.

<br>

---

## **strict mode의 적용**

전역의 선두 또는 함수 몸체의 선두에 `'use strict';` 를 추가한다. 전역에 추가하면 스크립트 전체에 적용된다.

<br>

---

## **전역에 strict mode를 적용하는 것은 피하자**

strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다. 특히 외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하는 것은 바람직하지 않다. 이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수 선두에 strcit mode를 적용한다.

<br>

---

## **함수 단위로 strict mode를 적용하는 것도 피하자**

함수 단위로 strict mode를 적용할 수도 있다. 어떤 함수는 적용하고, 어떤 함수는 적용하지 않는 것은 바람직하지 않으며 번거롭다. 또한, strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 문제가 발생할 수 있다.

<br>

---

## **strict mode가 발생시키는 에러**

- ### **암묵적 전역**

  선언하지 않은 변수를 참조하면 `ReferenceError`가 발생한다.

- ### **변수, 함수, 매개변수의 삭제**

  `delete` 연산자로 변수, 함수, 매개변수를 삭제하면 `SyntaxError`가 발생한다.

- ### **매개변수 이름의 중복**

  반복된 매개변수 이름을 사용하면 `SyntaxError`가 발생한다.

- ### **with 문의 사용**
  with 문을 사용하면 `SyntaxError`가 발생한다. `with` 문은 전달된 객체를 스코프 체인에 추가한다. `with` 문은 성능과 가독성이 낮아져 사용하지 않는 것이 좋다.

<br>

---

## **strict mode 적용에 의한 변화**

### **일반 함수의 this**

strict mode에서 함수를 일반 함수로서 호출하면 `this`에 `undefined`가 바인딩 된다. 생성자 함수가 아닌 일반 함수 내부에서는 `this`를 사용할 필요가 없기 때문이다.

### **arguments 객체**

strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```javascript
(function (a) {
  'use strict';
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다
  console.log(arguments); // { 0: 1, length: 1 }
})(1);
```

