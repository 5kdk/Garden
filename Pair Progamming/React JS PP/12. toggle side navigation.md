# **12. toggle side navigation**

- [**12. toggle side navigation**](#12-toggle-side-navigation)
  - [**상태 기반 렌더링**](#상태-기반-렌더링)
    - [**현상**(Facts)](#현상facts)
    - [**발견**(Discovery)](#발견discovery)
    - [**배운점**(Lessons Learned)](#배운점lessons-learned)
  - [**선언**(Declation)](#선언declation)

<br>

## **상태 기반 렌더링**

### **현상**(Facts)

- 바닐라 자바스크립트로 구현하였을 때에는 `isOpened`가 `false`인 상태로 정적 렌더링 된 후에, localsStorage의 값을 가져와 re-render가 되었다.
- 따라서 새로고침이나 페이지 이동 시에 Nav bar가 트랜지션이 발생하는 것이 보였다.

<br>

### **발견**(Discovery)

- 리액트로 `isOpened` 상태를 이용하여 구현하였다.
- `isOpened` 상태의 초기 값을 initialize 함수로 설정하고, 이를 기반으로 초기렌더링이 가능하다.
- 따라서 localStorage에 저장된 `isOpened` 상태가 `true`일 때, `false`로 정적 렌더링되고 `true` 상태로 트랜지션되는 것이 보이지 않으므로 transition을 무효화 시키는 preload 클래스를 이용할 필요가 없다.

<br>

### **배운점**(Lessons Learned)

- 리액트로 상태 관리를 했을 때 상태 기반으로 초기 렌더링이 가능해서 편리하다.
- useState 훅에 초기값을 전달할 때, localStorage에 저장된 값을 가져와 사용하는 경우에, localStorage에 해당 값이 없다면 `undefined`를 반환힌다. 따라서 `??` 널 병합 연산자를 사용하여 `undefined` 대신에 원하는 초기값을 전달할 수 있었다.

<br>

## **선언**(Declation)

- 리액트의 useState의 initialize 함수를 이용하여 정적렌더링의 문제점을 해결하자 ( initialize 함수의 중요성 )

<br>

---
