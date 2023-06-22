# **17. tree view🌳**

- [**17. tree view🌳**](#17-tree-view)
  - [**isOpen 상태 : TreeNode 컴포넌트에서 관리**](#isopen-상태--treenode-컴포넌트에서-관리)
    - [**현상**(Facts)](#현상facts)
    - [**발견**(Discovery)](#발견discovery)
    - [**배운점**(Lessons Learned)](#배운점lessons-learned)
  - [**TreeNode와 TreeLeafNode 분리**](#treenode와-treeleafnode-분리)
    - [**현상**(Facts)](#현상facts-1)
    - [**발견**(Discovery)](#발견discovery-1)
    - [**배운점**(Lessons Learned)](#배운점lessons-learned-1)
  - [**selecetedText 상태의 props drilling 방지**](#selecetedtext-상태의-props-drilling-방지)
    - [**현상**(Facts)](#현상facts-2)
    - [**발견**(Discovery)](#발견discovery-2)
    - [**배운점**(Lessons Learned)](#배운점lessons-learned-2)
  - [**선언**(Declation)](#선언declation)

<br>

## **isOpen 상태 : TreeNode 컴포넌트에서 관리**

### **현상**(Facts)

- tree의 각 노드들의 isOpen 상태를 상위 컴포넌트에서 관리했을 때, 클릭 이벤트 발생했을 때 전체 노드들이 re-render되는 현상이 있었다.

<br>

### **발견**(Discovery)

- `<TreeNode>`를 컴포넌트로 분리하면서 각 `<TreeNode>`가 상태를 관리할 수 있다는 것을 발견했다.

```js
const TreeNode = ({ name, subTree, isOpen }) => {
  const [isOpenState, setIsOpenState] = useState(isOpen);

  return (
      <Container>
        <Link
          onClick={e => {
            // ...
            setIsOpenState(!isOpenState);
          }}>
        {/* ...*/}
      </Container>
    );
};

```

<br>

### **배운점**(Lessons Learned)

- 바닐라JS 에서는 tree 배열을 상태로 관리하여 클릭이벤트가 발생하면 해당 노드의 `id`를 찾아 `isOpen` 값을 변경시켜주었지만, React에서는 전체 treeView가 re-render 되지 않고 click 이벤트가 발생한 컴포넌트에 대해서만 re-render가 일어나도록 구성할 수 있다는점을 배웠

<br>

## **TreeNode와 TreeLeafNode 분리**

### **현상**(Facts)

- treeView에는 children 여부에 따라 두 종류의 노드가 있다.
- 두 노드를 하나의 컴포넌트로 사용했을 때 받아야하는 props와 내부 로직이 복잡한 문제가 있었다.

1.  isOpenState는 `<TreeNode>`에서만 사용되는 상태이다.
2.  `<TreeLeafNode>`와 달리 `<TreeNode>`는 children으로 하위 컴포넌트를 생성해야 한다.
3.  클릭이벤트 발생 시
    - `<TreeLeafNode>` : setSelectedText를 통해 `<main>`에 title을 기록
    - `<TreeNode>` : children 노드들 렌더링 여부를 토글

<br>

### **발견**(Discovery)

- children 존재 여부에 따른 두 노드를 다른 컴포넌트로 관리하였을 때 로직의 복잡도를 낮추고 직관적인 코드 작성이 가능했다.

```js
<TreeNode key={id} name={name} subTree={children} isOpen={isOpen} />
```

```js
<TreeLeafNode key={id} name={name} />
```

  <br>

### **배운점**(Lessons Learned)

- 내부 로직이 많이 달라 하나의 컴포넌트로 구현하기에 복잡할땐, 컴포넌트의 재사용성도 좋지만 컴포넌트 복잡도를 낮추기 위해서 조건부 렌더링을 사용하는 방법이 나을 수도 있다는 점을 배웠다.

<br>

## **selecetedText 상태의 props drilling 방지**

### **현상**(Facts)

- 재귀적으로 컴포넌트를 그리는 과정에서 마지막 노드들에게 set 함수를 props로 전달해야하는데, 이를 위해서는 모든 노드에게 일일이 set 함수를 전달해야하는 문제가 있었다.

```jsx
const TreeNode = ({ name, subTree, isOpen }) => {
  const [isOpenState, setIsOpenState] = useState(isOpen);

  // ...
  return (
    <>
      // ...
      {subTree.map(({ id, name, children, isOpen }) =>
        children ? (
          <TreeNode key={id} name={name} subTree={children} isOpen={isOpen} setSelectedText={setSelectedText} />
        ) : (
          <TreeLeafNode key={id} name={name} setSelectedText={setSelectedText} />
        )
      )}
      // ...
    </>
  );
};
```

<br>

### **발견**(Discovery)

- 이 문제를 해결하기 위해 Context API를 사용하여, set 함수가 필요한 `<TreeLeafNode>` 컴포넌트에만 전달해줌으로써 코드의 가독성과 유지보수성을 높일 수 있었다.

```js
// SelectedContext
import { createContext } from 'react';

const SelectedContext = createContext();

export default SelectedContext;
```

```jsx
// TreeView
const TreeView = () => {
  const [selectedText, setSelectedText] = useState('');

  return (
    <SelectedContext.Provider value={[selectedText, setSelectedText]}>
    {...}
    </SelectedContext.Provider>;
  )
}
```

<br>

### **배운점**(Lessons Learned)

- 추가적인 라이브러리를 사용할 필요가 없이 React에 내장된 기능인 Context API와 useContext 훅을 이용하여 Props Drilling을 해결할 수 있었다.

```jsx
const TreeLeafNode = ({ name }) => {
  const [selectedText, setSelectedText] = useContext(SelectedContext);

  // ...
};
```

<br>

## **선언**(Declation)

- 부모 컴포넌트에서 상태를 관리하여 불필요한 re-render를 발생시키지 말고, 상태를 가져야하는 컴포넌트에서 관리하자.
- 컴포넌트를 한번 더 쪼개면 컴포넌트 내부 로직의 복잡도를 낮추며 더욱 직관적인 코드 작성이 가능한 경우도 있다. 컴포넌트 재사용에 너무 매몰되지 않도록 주의하자.
- 전역 상태관리 라이브러리 대신 Context API를 사용하면, 간단한 애플리케이션에서 추가적인 라이브러리 설치나 설정이 필요 없으며, Props Drilling을 피할 수 있다.

<br>

---
