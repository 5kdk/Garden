# **15. drag and drop 🪅**

- [**15. drag and drop 🪅**](#15-drag-and-drop-)
  - [**over 처리에 대한 고민**](#over-처리에-대한-고민)
    - [**현상**(Facts)](#현상facts)
    - [**발견**(Discovery)](#발견discovery)
    - [**배운점**(Lessons Learned)](#배운점lessons-learned)
  - [**선언**(Declation)](#선언declation)

<br>

## **over 처리에 대한 고민**

### **현상**(Facts)

- `dragTarget`이 `dropTarget`위를 통과 중이면 `dropTarget`은 over되어 배경색이 바뀌어야 하기 때문에 다양한 drag 이벤트들을 다뤄주어야 했다.

<br>

### **발견**(Discovery)

- over되고 있는 dropTarget 요소를 상태로 관리했지만, 다른 `dropTarget`을 통과중일 때 항상 `overTargetIdx`가 `null`이 되는 문제를 발견했다. (dragEnter 이벤트가 발생한 후에 dragLeave 이벤트가 발생했기 때문에)

  ```jsx
  // onDragEnter 이벤트가 발생하면 `overTargetIdx`를 해당 요소의 `idx`로 설정
  // onDragLeave 이벤트가 발생하면 `overTargetIdx`를 `null`로 설정

  onDragEnter={() => setOverTargetIdx(idx)}
  onDragLeave={() => setOverTargetIdx(null)}
  ```

<br>

- Solution 1 - dragLeave 이벤트 대신 drop 이벤트

  ```jsx
  onDragEnter={() => setOverTargetIdx(idx)}
  onDrop={() => {
    // ...
    setOverTargetIdx(null);
  }}
  ```

  - `dragTarget`이 다른 `dropTarget`위로 지나갈 때에는 잘 동작하는 것처럼 보이지만, Draggable List 밖으로 벗어났을 때, over된 요소가 유지되는 것을 확인할 수 있다.

<br>

- Solution 2 - onDragLeave 이벤트에서 update 함수 사용

  ```jsx
  onDragLeave={() => setOverTargetIdx(overTarget => (overTarget === idx ? null : overTarget))}
  ```

  - dragLeave 이벤트가 발생했을 때 직전 `overTargetIdx` 상태값을 확인하고, 현재 `idx`와 같은 값이라면 (다른 요소로 dragEnter 이벤트가 발생하지 않은 경우) `null`로 설정하도록 수정하였다.
  - 만약 해당 요소의 `idx`와 `overTargetIdx`가 다른 값이라면 `dragTarget`이 다른 요소 위를 통과중이므로` overTarget` 그대로 유지할 수 있었다.

<br>

- 위와 같이 `<Swappable>`컴포넌트에서 `overTarget` 상태를 관리할 때, `overTarget`이 변경될 때마다 하위 컴포넌트들 전체에 re-render가 발생했다.
- 하위 컴포넌트 `<Draggable>`에서 각각 `isOvered` 상태를 관리하여 상태 변경시 해당 컴포넌트만 re-render 가능하다.

```jsx
const Draggable = ({ value, handleDragStart, swap, isCorrect }) => {
  const [isOvered, setIsOvered] = useState(false);

  return (
    <Container
      draggable="true"
      over={isOvered}
      onDragStart={handleDragStart}
      onDragEnter={() => setIsOvered(true)}
      onDragLeave={() => setIsOvered(false)}
      onDragOver={e => e.preventDefault()}
      onDrop={() => {
        swap();
        setIsOvered(false);
      }}>
      <SubTitle isCorrect={isCorrect}>{value}</SubTitle>
      <AiOutlineMenu />
    </Container>
  );
};
```

<br>

### **배운점**(Lessons Learned)

- 상위 컴포넌트가 상태를 가지고 있으면 상태가 변경될 때마다 자신을 포함한 하위 컴포넌트들이 re-render 된다.
- 불필요한 렌더링이 일어나는지 확인하고 상태 관리를 해야하는 컴포넌트를 잘 파악해야 한다.

<br>

## **선언**(Declation)

- 상위 컴포넌트에서 상태를 관리할 필요가 있는지 잘 판단하고 어느 컴포넌트에서 상태를 관리해야 하는지 고려하면서 코딩을 하자.
- 이번 미션의 이벤트처럼 re-render가 여러번 발생하는 경우에 re-render가 꼭 필요한 컴포넌트들만 re-render가 되도록 상태를 가져야할 컴포넌트를 잘 고려하자.

<br>

---
