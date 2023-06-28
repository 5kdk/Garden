# useMemo, useCallback 최적화가 꼭 필요할까?

_(2022.12 블로그 글 옮김)_

`useCallback()`은 함수를 메모이제이션(memoization)하기 위해서 사용되는 훅이다. React의 `useCallback`를 `useEffect`와 함께 사용하여 여러방면으로 효율적인 코드를 구성할 수 있는 방법이 있다. `useCallback`은 `useEffect`와 생긴 것은 똑같지만, 역할은 조금 다르다.

<br>

### 예제로 보는 useEffect 심화 활용

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

첫 번째 인자로 콜백함수를 넣어주고 호출해주고 있고, 두 번째로 인자로 의존성 배열을 넣어준다. `a`나 `b`가 바뀌었을 때만 `useCallback`의 콜백 함수를 새로 생성하고, 그렇지 않을 경우 콜백 함수를 생성하지 않는다.

<br>

#### with useEffect

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);

useEffect(() => {
  memoizedCallback();
}, [memoizedCallback]);
```

의존성 배열에도 `memoizedCallback`이 있고, 콜백에서도 `memoizedCallback`을 호출하고 있다.

`memoizedCallback`는 `a`, `b`에 의존적이므로, `a`와 `b`가 변경이 생겼을 때만 생성될 것이고, `useEffect`는 `memoizedCallback`이 새로 생성되었을 때 `memoizedCallback`을 호출할 것이다.

```jsx
useEffect(() => {
  doSomething(a, b);
}, [a, b]);
```

<br>

**사실, 위 코드는 앞서본 코드와 같은 형태를 띠게 된다. 훨씬 간단해 보이는데 useCallback은 왜 사용할까?**

**problem**

- useEffect에서 하는 행동이 많아질수록 코드가 복잡해진다
- dependency array가 엄청 길어진다
- 나중에 유지보수를 하려하면 헷갈리게 될 수 있다

**solution**

- useEffect는 우리가 원하는 side effect를 실행만 해주고,  
  useCallback에서 실제 해야 하는 것들을 선언해주면 코드의 분리가 가능하다
- 콜백 함수의 구체적인 역할을 이름을 통해 명시적으로 확인할 수 있다
- 코드를 정리하고 가독성을 높여서 유지보수를 수월하게 해 줄 수 있다

<br>

#### state가 바뀔 때마다 데이터를 받아와야 하는 경우

**only useEffect**

```jsx
useEffect(() => {
  const getDate = async () => {
    const response = await fetch('some url');
    setData(response.data);
  };
  getData();
}, [someDeps]);
```

`useEffect`의 콜백함수로 Promise를 반환하는 비동기 함수를 줄 수 없다. 때문에, `getData`라는 비동기 함수를 `useEffect`가 실행될 때마다 생성해주고, 그 안에서 데이터를 가져오고 `setData`를 해주게 된다. 그리고 만든 `getData`라는 함수를 실행하게 된다. `useEffect`가 실행될 때마다 매번 함수가 생성되고 실행되니 매우 비효율적이다.

<br>

**useEffect + useCallback**

```jsx
const getData = useCallback(async () => {
  const response = await fetch('some url');
  setData(response.data);
}, [someDeps]);

useEffect(() => {
  getData();
}, [getData]);
```

`useCallback`을 활용하면, `useCallback`안에서 데이터를 가져오는 콜백 함수를 `getData`에 넣어주고, `useEffect`안에서는 `getData`만 호출을 해주면 코드가 훨씬 깔끔해지고, 로직도 분리되며 가독성도 올라가는 효과를 얻을 수 있다.
