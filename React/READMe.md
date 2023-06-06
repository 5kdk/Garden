# 리액트 관련 자료 모음

### 링크로 이동 `Ctrl + Click`

### React

- [useState](https://react.dev/reference/react/useState)
- [useEffect](https://react.dev/reference/react/useEffect)
- [useRef](https://beta.reactjs.org/reference/react/useRef)
  - [Referencing Values with Refs](https://beta.reactjs.org/learn/referencing-values-with-refs)
  - [Manipulating the DOM with Refs](https://beta.reactjs.org/learn/manipulating-the-dom-with-refs)
  - [ref callback](https://beta.reactjs.org/reference/react-dom/components/common#ref-callback) - [Avoiding useEffect with callback refs](https://tkdodo.eu/blog/avoiding-use-effect-with-callback-refs)
- [useCallback](https://beta.reactjs.org/reference/react/useCallback)
- [useMemo](https://beta.reactjs.org/reference/react/useMemo)
  - [When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback)
  - [How to useMemo and useCallback: you can remove most of them](https://www.developerway.com/posts/how-to-use-memo-use-callback)
- reactive (derived) state
  - [What is derived state in React, and why is it important?](https://stackoverflow.com/questions/58288286/what-is-derived-state-in-react-and-why-is-it-important)
- [synthetic event](https://beta.reactjs.org/reference/react-dom/components/common#react-event-object)
- [Why you need keys for collections in React](https://paulgray.net/keys-in-react/)
- [Applying Classes Conditionally in React](https://www.pluralsight.com/guides/applying-classes-conditionally-react)
- [controlled component](https://beta.reactjs.org/reference/react-dom/components/input#controlling-an-input-with-a-state-variable)
- [uncontrolled component](https://ko.reactjs.org/docs/uncontrolled-components.html)
- [When to break up a component into multiple components](https://kentcdodds.com/blog/when-to-break-up-a-component-into-multiple-components)
- [lifting state up](https://beta.reactjs.org/learn/sharing-state-between-components)
  - 컴포넌트의 역할 분담이 불명확해지고, 결합도가 높아진다.
- [props drilling](https://beta.reactjs.org/learn/passing-data-deeply-with-context)
- [Reusing Logic with Custom Hooks](https://beta.reactjs.org/learn/reusing-logic-with-custom-hooks)
- [fetch data](https://poiemaweb-react.notion.site/46aaaaffa07246f9ab9673034947119b)
- [Optimistic Vs Pessimistic operations in REACT](https://chibueze.hashnode.dev/optimistic-vs-pessimistic-operations-in-react)
- [Extracting State Logic into a Reducer](https://react.dev/learn/extracting-state-logic-into-a-reducer)
- [Passing Data Deeply with Context](https://react.dev/learn/passing-data-deeply-with-context)
- [Scaling Up with Reducer and Context](https://react.dev/learn/scaling-up-with-reducer-and-context)
- [Context API 렌더링 이슈](https://jungpaeng.tistory.com/58)
- Context
  - [createContext](https://beta.reactjs.org/reference/react/createContext)
  - [useContext](https://beta.reactjs.org/reference/react/useContext)
- [useReducer](https://beta.reactjs.org/reference/react/useReducer)
- [상태 관리](https://maeng2418.github.io/react/state_management/)

<br>

### Styled Component

- [global styles](https://styled-components.com/docs/api#createglobalstyle)
- [adapting based on props](https://styled-components.com/docs/basics#adapting-based-on-props)
- [attrs](https://styled-components.com/docs/basics#attaching-additional-props)
- [Best ways to style a React-JS application](https://blog.devgenius.io/best-ways-to-style-a-react-js-application-c818b71f6341)

<br>

### Vite

- [vite proxy](https://vitejs-kr.github.io/config/server-options.html#server-proxy)
  - [CORS](https://velog.io/@sonwj0915/CORS%EC%99%80-Proxy%EB%A5%BC-%ED%99%9C%EC%9A%A9%ED%95%98%EC%97%AC-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0)

<br>

### Recoil

- [Handling global state in React in 2022](https://www.osedea.com/en/blog/handling-global-state-in-react-in-2022)
- [Recoil - 또 다른 React 상태 관리 라이브러리?](https://ui.toast.com/weekly-pick/ko_20200616)
- [recoil](https://recoiljs.org/ko/)(global state)

<br>

### React Query

- [카카오페이 프론트엔드 개발자들이 React Query를 선택한 이유](https://tech.kakaopay.com/post/react-query-1/)
- [react query (server state)](https://tanstack.com/query/latest)
  - [Important Defaults](https://tanstack.com/query/latest/docs/react/guides/important-defaults)
  - [Queries](https://tanstack.com/query/latest/docs/react/guides/queries)
  - [mutations](https://tanstack.com/query/latest/docs/react/guides/mutations)
  - [optimistic updates](https://tanstack.com/query/v4/docs/react/guides/optimistic-updates)
    - [react-query optimistic update시 데이터 꼬임 방지](https://velog.io/@mskwon/react-query-cancel-queries)
- [Suspense와 선언적으로 Data fetching처리](https://fe-developers.kakaoent.com/2021/211127-211209-suspense/)
- [React에서 선언적으로 비동기 다루기](https://jbee.io/react/error-declarative-handling-1/)
- [React Query와 함께 Concurrent UI Pattern을 도입하는 방법](https://tech.kakaopay.com/post/react-query-2/#react-query%EC%99%80-%ED%95%A8%EA%BB%98-suspense%EC%99%80-error-boundary-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
- [<Suspense>](https://react.dev/reference/react/Suspense)
- [Catching rendering errors with an error boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary)
- [react-query]()
  - [Suspense](https://tanstack.com/query/v4/docs/react/guides/suspense)
  - [react-error-boundary](https://github.com/bvaughn/react-error-boundary#readme)
  - [QueryErrorResetBoundary](https://tanstack.com/query/v4/docs/react/reference/QueryErrorResetBoundary/) [useQueryErrorResetBoundary](https://tanstack.com/query/v4/docs/react/reference/useQueryErrorResetBoundary)
