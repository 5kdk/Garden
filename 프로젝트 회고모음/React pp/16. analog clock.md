# **16. analog clock🕝**

- [**16. analog clock🕝**](#16-analog-clock)
  - [**useEffect를 이용한 setInterval 실행시 주의점들**](#useeffect를-이용한-setinterval-실행시-주의점들)
    - [**현상**(Facts)](#현상facts)
    - [**발견**(Discovery)](#발견discovery)
    - [**배운점**(Lessons Learned)](#배운점lessons-learned)
  - [**선언**(Declation)](#선언declation)

<br>

## **useEffect를 이용한 setInterval 실행시 주의점들**

### **현상**(Facts)

- 1초 간격으로 시침, 분침, 초침이 모두 일정 각도로 회전해야 한다.
- date 객체를 상태(`time`)로 관리하였고, `setInterval`를 이용하여 1000ms 마다 상태를 변경해주었다.

<br>

### **발견**(Discovery)

- 상태가 변경되면 re-render가 발생하며 re-render 될 때마다 `tick`함수와 `setInterval` 함수가 새롭게 정의되고 실행된다.
- 따라서 `useEffect`를 이용하여 초기 렌더링에만 `setInterval` 함수가 실행될 수 있도록 구현하였다.

  ```js
  useEffect(() => {
    const tick = () => setTime(new Date());
    const intervalId = setInterval(tick, 1000);
    return () => {
      clearInterval(intervalId);
    };
  }, []);
  ```

  <br>

### **배운점**(Lessons Learned)

<br>

## **선언**(Declation)

- 1000ms 간격으로 time 상태가 변경되면서 re-render가 발생하며, useEffect를 사용하지 않으면 re-render가 될 때마다 새로운 setInterval이 실행된다.
- 이와 같은 리액트 상태의 특징(변경될 때마다 re-render 발생)을 잘 알고 훅을 사용하자.

<br>

---
