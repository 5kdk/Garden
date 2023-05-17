# Mantine Rules (2023.04)

## 👀컴포넌트

1. mantine 컴포넌트를 활용하고 각 컴포넌트의 docs props를 참고하여 `style props`로 해결한다.

   > [Style props](https://mantine.dev/styles/style-props/)

2. 대표 props

   ```jsx
   import { Box } from '@mantine/core';

   // margin-top: theme.spacing.xs
   <Box mt="xs" />

   // margin-top: theme.spacing.md * -1
   <Box mt="-md" />

   // margin-top: auto
   <Box mt="auto" />

   // margin-top: 1rem
   <Box mt={16} />

   // margin-top: 5rem
   <Box mt="5rem" />

   // color: theme.colors.blue[theme.fn.primaryShade()]
   <Box c="blue" />

   // background: theme.colors.orange[1]
   <Box bg="orange.1" />

   // color: theme.colorScheme === 'dark' ? theme.colors.dark[2] : theme.colors.gray[6]
   <Box c="dimmed" />

   // background: #EDFEFF
   <Box bg="#EDFEFF" />

   // background: rgba(0, 34, 45, 0.6)
   <Box bg="rgba(0, 34, 45, 0.6)" />
   ```

3. props에 `{}`로 감싸주면 px기준 값이 rem으로 변환되기 때문에 이를 활용한다.
   ```jsx
   // X
   <Box w="1rem" />
   <Box w="16px" />
   // O
   <Box w={16} />
   ```
4. props color를 사용할 경우 `useMantineTheme()` 다음과 같이 사용한다.

   ```jsx
   const TempLoader = () => {
     const theme = useMantineTheme();
     const { colorScheme } = useMantineColorScheme();
     const dark = colorScheme === 'dark';

     return (
       <Conatiner>
         <Loader color={dark ? theme.colors.gray[0] : theme.colors.dark[8]} />
       </Conatiner>
     );
   };
   ```

5. style props로 진행할 수 없는 스타일은 `styled` 함수를 사용한다.
6. `<MantineProvider />`로 theme을 전달하고 있기때문에 styled 컴포넌트에서 접근할 수 있다.
   ```jsx
   // example
   const Temp = styled(Container)`
   	// ...
   	background-color: `${(({theme}) => theme.colorScheme === 'dark' ? theme.colors.dark[8] : theme.colors.gray[0])}`;
   `
   ```
7. styled 컴포넌트 내부에서는 `rem()` 함수가 아닌 px to rem(vscode extention)을 활용하여 px을 rem으로 변환한다.
8. styled 컴포넌트에서는 css variable을 활용한다.

   ```jsx
   // example
   const Temp = styled(Container)`
     // ...
     padding: var(--mantine-spacing-md);
   `;
   ```

9. mantine의 중첩된 컴포넌트(nested elements, ex: `Modal`, `Slide` 등)은 docs에 있는 props를 꼭 살펴보고 사용한다. static selector로 컴포넌트 Styles API에 접근할 수 있다.

   > [Styled components](https://mantine.dev/styles/styled/)  
   > [MantineProvider](https://mantine.dev/theming/mantine-provider/)

10. Mantine UI에 있는 예제 코드를 활용하고자 할 경우, 위 컨벤션에 만족하도록 변형한다.

<br>

## 🖼️테마

1. 컴포넌트의 props로 해결 가능한 경우 `useMantineColorScheme()` 함수를 호출하여 다음과 같이 조건부 처리한다.

   ```jsx
   // example
   const Temp = () => {
     const { colorScheme } = useMantineColorScheme();
     const dark = colorScheme === 'dark';

     return (
       <Container>
         <Button color={dark ? 'gray' : 'black'} />
       </Container>
     );
   };
   ```

2. props로 해결이 어려운 경우 (ex: backgroundColor, 컬러를 theme.colors의 key만 허용하는 컴포넌트 등)

   ```jsx
   // example
   const Temp = styled(Container)`
   	// ...
   	background-color: `${(({theme}) => theme.colorScheme === 'dark' ? theme.colors.dark[8] : theme.colors.gray[0])}`;
   `
   ```

<br>

## 🔌React Router

- 라우팅시엔 다음과 같이 사용한다.

  ```jsx
  import { Link } from 'react-router-dom';
  import { Button } from '@mantine/core';

  const Temp = () => (
    <Button component={Link} to="/react-router">
      React router link
    </Button>
  );
  ```

  > [Polymorphic components](https://mantine.dev/guides/polymorphic/)

<br>

## 반응형 컴포넌트

1. 컴포넌트의 props로 해결가능한 경우 조건부 렌더를 할 경우 `use-media-query` 를 사용한다.

   > [use-media-query](https://mantine.dev/hooks/use-media-query/)

2. styled 컴포넌트로 미디어 쿼리를 작성한다.
