# **13. tic tac toe**

- [**13. tic tac toe**](#13-tic-tac-toe)
  - [**상태와 파생상태**](#상태와-파생상태)
    - [**현상**(Facts)](#현상facts)
    - [**발견**(Discovery)](#발견discovery)
    - [**배운점**(Lessons Learned)](#배운점lessons-learned)
  - [**선언**(Declation)](#선언declation)

<br>

## **상태와 파생상태**

### **현상**(Facts)

- 바닐라JS로 구현할 때와는 달리 리액트에서는 상태가 변경되면 re-render가 발생하기 때문에 상태로 관리할 값들을 잘 정해야한다.
- UI에 영향을 끼치는 값
  - `player`: 현재 플레이어
  - `board`: 게임 보드 상황 배열
  - `isTicTacToe`: 누군가의 승리가 결정났는지 여부
  - `isDraw`: 무승부로 게임이 종료되었는지 여부

<br>

### **발견**(Discovery)

- 현재 4개의 값들이 UI에 영향을 미친다
- 그러나 4개의 값 모두 상태로 관리할 필요가 없다.
- `isTicTacToe`와 `isDraw`의 경우, `board`로 계산할 수 있는 값이다.
  - `isTicTacToe` : `board`에서 `winConditions`에 만족하는 플레이어가 있는지 여부
  - `isDraw`
    - TRUE : `board`의 모든 칸이 클릭되어있고 `isTicTacToe`가 `false`
    - FALSE : `board`의 빈칸이 존재하거나 `isTicTacToe`가 `true`

```js
// 파생 상태
const isTicTacToe = winConditions.some(([A, B, C]) => board[A] && board[A] === board[B] && board[B] === board[C]);
const isDraw = !isTicTacToe && board.every(item => item !== '');
```

- 추가적으로 isPlaying(게임이 진행 중인지 여부)을 파생상태로 관리하였다.

```js
const isPlaying = !(isTicTacToe || isDraw);
```

- 아래와 같이 조건에 따라 출력해야 하는 Header의 text를 더 명시적이고 가독성 있게 작성하기 위해 isPlaying을 파생상태로 관리하였다.

```js
// before
<Header>
  {isTicTacToe ? `Winner is ${player === 'O' ? 'X' : 'O'}` : isDraw ? 'Draw' : `Next Player: ${player}`}
</Header>

// after
<Header>
  {isTicTacToe && `Winner is ${player === 'O' ? 'X' : 'O'}`}
  {isDraw && 'Draw'}
  {isPlaying && `Next Player: ${player}`}
</Header>
```

  <br>

### **배운점**(Lessons Learned)

- UI에 영향을 주는 모든 값들을 상태로 관리할 필요가 없다.
- 값들이 다른 상태에 의해서 파생된 값인지 잘 파악할 수 있어야 한다.
- 따라서 player와 board만 상태로 관리하였으며, 나머지 두 값 isTicTacToe와 isDraw는 파생상태이므로 상태로 관리하지 않았다.

<br>

## **선언**(Declation)

- 상태와 파생 상태를 정확히 이해하고, 상태를 적극적으로 활용하여 유용한 값(상태를 활용한 파생 상태)을 만들어 불필요한 상태 사용을 최소화하자.

<br>

---
