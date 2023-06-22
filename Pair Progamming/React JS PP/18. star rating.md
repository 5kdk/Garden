# **18. star rating⭐**

- [**18. star rating⭐**](#18-star-rating)
  - [**버블링되지 않는 mouseEnter, mouseLeave**](#버블링되지-않는-mouseenter-mouseleave)
    - [**현상**(Facts)](#현상facts)
    - [**발견**(Discovery)](#발견discovery)
    - [**배운점**(Lessons Learned)](#배운점lessons-learned)

<br>

- mouseLeave, mouseEnter ( 버블링)

<br>

## **버블링되지 않는 mouseEnter, mouseLeave**

### **현상**(Facts)

- 바닐라 JS로 구현했을 때에는 mouseOver 이벤트의 버블링을 이용하여 상위 요소에 이벤트 핸들러를 등록하였다.

<br>

### **발견**(Discovery)

- 리액트로 구현할 때는 컴포넌트 각각에 이벤트 핸들러를 등록할 수 있으므로 버블링이 필요하지 않다.

```js
  onMouseEnter={() => setHoveredRating(i + 1)}
  onMouseLeave={() => setHoveredRating(0)}
```

<br>

### **배운점**(Lessons Learned)

- 컴포넌트 별 이벤트 핸들러 등록이 편리하다.
- 따라서 상위 컴포넌트에서 버블링을 이용하여 이벤트를 캐치할 필요가 없다.

<br>

<!-- ## **선언**(Declation)

-

<br> -->

---

<!--

```
react-dom.development.js:86 Warning:

Received `false` for a non-boolean attribute `hovered`.

If you want to write it to the DOM, pass a string instead: hovered="false" or hovered={value.toString()}.

If you used to conditionally omit it with hovered={condition && value}, pass hovered={condition ? value : undefined} instead.
```

1.  React에서 조건부로 props를 설정할 때, `hovered={condition && value}` 대신 `hovered={condition ? value : undefined}`를 사용하는 것이 권장됩니다. 이는 후자를 사용할 경우 경고 메시지가 발생할 수 있기 때문입니다.

경고 메시지는 "Received false for a non-boolean attribute hovered."와 같이 표시됩니다. 이는 조건이 false일 때는 hovered prop이 설정되지 않기 때문에, prop의 값이 boolean(`true` 또는 `false`)이 되기 때문입니다. 그러나 조건이 `false`일 경우, hovered prop은 `undefined`로 전달됩니다.

React 프레임워크에서 prop의 값이 없을 때는 일반적으로 boolean `false`로 간주되는데, 이는 prop이 다른 데이터 타입을 예상할 때 문제를 일으킬 수 있습니다. 이러한 문제를 방지하기 위해 조건이 `false`일 때는 명시적으로 `undefined`를 전달하는 것이 좋습니다. 이렇게 하면 prop이 정의되지만 값이 없는 상태가 되며, 경고 메시지를 방지할 수 있습니다.

예를 들어, `hovered={isHovered && 'red'}` 대신 `hovered={isHovered ? 'red' : undefined}`와 같이 사용할 수 있습니다. 이렇게 하면 isHovered가 true일 때는 'red' 값을 갖는 hovered prop이 전달되고, false일 때는 값이 없는 상태로 전달됩니다. 이는 위에서 언급한 경고 메시지를 방지하며, hovered prop이 값이 없어도 정의되도록 보장합니다.

2.  SVG 요소는 일반 HTML 요소와는 다른 방식으로 동작하기 때문에, React에서 일부 속성을 처리하는 방식도 다릅니다.

SVG 요소는 다른 HTML 요소와는 달리 일반적으로 CSS 스타일을 적용하는 대신, 속성(attributes)을 사용하여 스타일을 적용합니다. 따라서, SVG 요소에서 일부 속성은 불리언(boolean) 타입이 아닌 다른 타입으로 사용됩니다.

React에서 불리언 타입이 아닌 속성을 다룰 때, 일부 속성이 전달되지 않을 경우 오류를 발생시키거나 예기치 않은 동작을 할 수 있습니다. 이러한 문제를 방지하기 위해, React에서 SVG 요소에서는 "hovered={condition && value}" 대신 "hovered={condition ? value : undefined}"와 같은 방식으로 속성을 처리하는 것이 권장됩니다.

따라서, 일부 요소에서는 이러한 주의를 강조하지 않아도 되는 경우도 있지만, SVG 요소에서는 이러한 주의가 필요한 경우가 많습니다.

-->
