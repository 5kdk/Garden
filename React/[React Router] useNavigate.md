# useNavigate

https://reactrouter.com/en/main/hooks/use-navigate

### 개요:

[원티드 프리온보딩 사전과제](https://github.com/5kdk/wanted-pre-onboarding-frontend/tree/main)를 진행하며 여러 리다이렉트 작업이 필요했다.

`Root` 컴포넌트 `/` 경로 접근시 `SignIn` 페이지로 리다이렉트를 하고 있었으며 라우팅 시 `SignIn`과 `Todo` 페이지는 로그인 여부에 따른 리다이렉트를 하는 `AuthenticationGuard`가 감싸고 있다.

`useNavigate`를 사용할 때 뒤로가기가 제대로 되지 않는 문제가 발견되었고, 이를 해결하기 위해 `useNavigate`를 좀 더 꼼꼼하게 살펴보았다.

<br>

### 톺아보기:

React Router의 `useNavigate` 훅은 React 컴포넌트에서 프로그래밍 방식으로 라우팅을 제어할 수 있게 해주는 도구.

`useNavigate`를 사용하면 URL을 변경하거나 특정 경로로 이동할 수 있다. `useNavigate` 훅은 React Router v6부터 도입되었다.

`useNavigate` 훅을 사용하기 위해 먼저 React Router를 설치하고 `useNavigate`를 가져와야 한다.

```js
import { useNavigate } from 'react-router-dom';
```

`useNavigate` 훅은 라우터 컴포넌트 내부에서 사용되어야 한다. 예를 들어, 다음과 같이 컴포넌트를 정의할 수 있다.

```jsx
import { useNavigate } from 'react-router-dom';

const MyComponent = () => {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/other-route');
  };

  return (
    <div>
      <h1>My Component</h1>
      <button onClick={handleClick}>Go to Other Route</button>
    </div>
  );
};
```

위의 예제에서 `handleClick` 함수에서 `navigate` 함수를 호출하면 URL을 변경하고 `/other-route`로 이동한다.

`useNavigate`를 사용하여 다양한 옵션을 지정할 수도 있다. 이를 통해 리다이렉트, 상태 변경, 쿼리 매개변수 전달 등 다양한 작업을 수행할 수 있다.

```jsx
import { useNavigate } from 'react-router-dom';

const MyComponent = () => {
  const navigate = useNavigate();

  const handleClick = () => {
    navigate('/other-route', { state: { id: 123 }, replace: true });
  };

  return (
    <div>
      <h1>My Component</h1>
      <button onClick={handleClick}>Go to Other Route</button>
    </div>
  );
};
```

위의 예제에서는 `navigate` 함수의 두 번째 인수로 객체를 전달하여 옵션을 설정할 수 있는데, `state` 프로퍼티를 사용하여 상태를 전달하고, `replace` 프로퍼티를 `true`로 설정하여 이동한 후에 브라우저의 기록에 남지 않도록 할 수 있다.

<br>

`state`와 `replace` 프로퍼티를 활용하기 좋은 상황을 예시로 들자면:

1. 게시글 목록이 있는 페이지에서 사용자가 특정 게시글을 클릭하면 해당 게시글의 상세 페이지로 이동하고, 상세 페이지에서는 게시글의 ID와 함께 추가적인 데이터를 사용해야 할 때 `state` 프로퍼티를 활용할 수 있다.

   ```jsx
   import { useNavigate } from 'react-router-dom';

   const PostList = () => {
     const navigate = useNavigate();

     const handlePostClick = postId => {
       // 게시글 ID와 추가 데이터를 함께 전달하여 상세 페이지로 이동
       navigate(`/post/${postId}`, {
         state: { id: postId, additionalData: 'Some data' },
       });
     };

     return (
       <div>
         <h1>Post List</h1>
         <ul>
           <li onClick={() => handlePostClick(1)}>Post 1</li>
           <li onClick={() => handlePostClick(2)}>Post 2</li>
           <li onClick={() => handlePostClick(3)}>Post 3</li>
         </ul>
       </div>
     );
   };
   ```

   ```jsx
   import { useLocation } from 'react-router-dom';

   const PostDetail = () => {
     const location = useLocation();
     const { id, additionalData } = location.state;

     return (
       <div>
         <h1>Post Detail</h1>
         <p>ID: {id}</p>
         <p>Additional Data: {additionalData}</p>
       </div>
     );
   };
   ```

2. 로그인 후에 사용자를 로그인 페이지로 돌아갈 수 없도록 하기 위해, 브라우저의 기록에서 이전 로그인 페이지를 대체하면 사용자 경험을 올릴 수 있다.(뒤로가기를 눌러도 로그인 페이지로 가지 않음)

   ```js
   const LoginPage = () => {
     const navigate = useNavigate();

     const handleLogin = () => {
       // 로그인 처리 로직...

       // 로그인 성공 후 대시보드 페이지로 이동 (replace: true 옵션 사용)
       navigate('/dashboard', { replace: true });
     };

     return (
       <div>
         <h1>Login Page</h1>
         <button onClick={handleLogin}>Login</button>
       </div>
     );
   };
   ```

이와 같이 `useNavigate`를 사용하여 React 컴포넌트에서 프로그래밍 방식으로 라우팅을 제어할 수 있으며 `useNavigate`의 다양한 옵션을 활용하면 더욱 유연하고 풍부한 사용자 경험을 제공할 수 있다.

<br>

### 결론:

`useNavigate`, `Navigate`(useNavigate를 둘러싼 컴포넌트 래퍼)는 `replace` 프로퍼티를 사용하여 브라우저의 기록에 새로운 항목을 추가하지 않고 이전 항목을 대체할 수 있다.

결국 리다이렉트시 `{ replace: true }`옵션을 주지 않아 뒤로가기 문제가 생겼던 것이다.

로그인 또는 인증과 관련된 경로 전환과 같이 이전 페이지로 돌아가지 않아야 하는 상황에서 `replace` 프로퍼티를 활용하자.
