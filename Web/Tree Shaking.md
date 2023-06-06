# **Tree Shaking**

### **개요**

트리 셰이킹은 자바스크립트 컨텍스트에서 데드 코드 제거를 설명하기 위해 일반적으로 사용되는 용어다.

나무를 흔들면 죽은 잎사귀들이 떨어지는 모습에 착안해 Tree-shaking이라고 부른다.

- 트리 셰이킹은 가져오기 및 내보내기 문을 사용하여 JavaScript 파일 간에 사용하기 위해 코드 모듈을 내보내고 가져오는지 감지한다.

- 최신 JavaScript 애플리케이션에서는 모듈 번들러(예: 웹팩 또는 롤업)를 사용하여 여러 JavaScript 파일을 단일 파일로 번들링할 때 데드 코드를 자동으로 제거한다. 이는 깔끔한 구조와 최소한의 파일 크기 등 프로덕션에 바로 사용할 수 있는 코드를 준비하는 데 중요하다.

- 타입스크립트의 TSC가 자바스크립트로 컴파일 할 때 타입 관련 코드를 제거하는 작업

<br>

> **참고**
>
> [MDN Tree Shaking](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking)  
> [TypeScript enum을 사용하지 않는 게 좋은 이유를 Tree-shaking 관점에서 소개합니다.](https://engineering.linecorp.com/ko/blog/typescript-enum-tree-shaking)
