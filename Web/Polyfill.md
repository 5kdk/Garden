# **Polyfill**

### **개요**

- 폴리필(Polyfill)은 웹 개발에서 사용되는 개념으로, 웹 플랫폼의 표준 기능을 구현하지 않은 오래된 브라우저나 환경에서도 동작할 수 있도록 기능을 제공하는 코드 조각을 말한다.
- 웹 플랫폼은 계속해서 발전하고 새로운 표준과 기능이 추가된다. 하지만 오래된 버전의 브라우저는 최신 표준을 지원하지 않을 수 있다.
- 이러한 경우에 폴리필은 브라우저에 없는 특정 기능을 구현하여 그 기능을 사용할 수 있도록 한다.
- 폴리필은 JavaScript로 작성되며, 웹 개발자가 특정 기능이 지원되지 않는 브라우저에서도 동작하도록 코드를 추가할 수 있게 한다.

- 폴리필은 두 가지 주요 형태로 나뉜다:

  - 기능 폴리필(Functionality polyfills): 이러한 폴리필은 웹 플랫폼의 새로운 기능을 지원하지 않는 브라우저에 새로운 기능을 추가한다. 예를 들어, 오래된 브라우저에서 `fetch` API를 사용하려면 `fetc`h API를 지원하지 않는 브라우저에서도 동작할 수 있는 폴리필을 추가해야 한다.
  - 문법 폴리필(Syntax polyfills): 이러한 폴리필은 오래된 버전의 JavaScript 엔진에서 최신 JavaScript 문법을 사용할 수 있도록 도와준다. 예를 들어, 오래된 버전의 브라우저에서 `const`나 `let`과 같은 블록 범위 변수 선언을 사용하려면 폴리필을 사용하여 이를 지원해야 한다.

- 폴리필은 일반적으로 JavaScript 라이브러리나 프레임워크와 함께 사용된다. 라이브러리나 프레임워크는 자체적으로 폴리필을 제공할 수도 있다. 또한, 개발자들은 필요한 폴리필을 찾아서 사용하거나 직접 작성할 수도 있다.

- 폴리필을 사용하여 오래된 브라우저와 호환성을 유지하면서 최신 웹 기술을 활용할 수 있다. 이는 웹 개발자들에게 플랫폼의 제한을 극복하고 모든 사용자에게 동일한 사용자 경험을 제공하는 데 도움을 준다.

<br>

### **라이브러리 예**

1. **Babel**  
   Babel은 JavaScript 컴파일러이면서 동시에 폴리필도 제공하는 유명한 도구. Babel을 사용하면 최신 JavaScript 문법을 사용하고, 브라우저 호환성을 위해 필요한 폴리필을 자동으로 적용할 수 있다.

2. **Polyfill.io**  
   Polyfill.io는 필요한 폴리필만 동적으로 제공하는 클라우드 기반 서비스. 이 서비스는 브라우저의 user-agent를 기반으로 필요한 폴리필을 자동으로 선택하고 제공해준다. 이렇게 하면 필요한 폴리필만 다운로드하여 사용자에게 최적화된 번들을 제공할 수 있다.

3. **core-js**  
   core-js는 ECMAScript 표준에 기반한 폴리필 라이브러리. 다양한 기능과 문법의 폴리필을 제공하며, 필요한 폴리필만 선택적으로 사용할 수 있다.

4. **polyfill.io CDN**  
   polyfill.io CDN은 Polyfill.io 서비스의 CDN 버전으로, 필요한 폴리필을 동적으로 제공하는 기능을 제공한다. CDN을 사용하면 자체 서버에 폴리필을 호스팅할 필요없이 간편하게 사용할 수 있다.

5. **Modernizr**  
   Modernizr는 브라우저에서 지원되는 기능을 감지하는 JavaScript 라이브러리. Modernizr을 사용하여 특정 기능이 지원되지 않는 경우에 대비하여 폴리필을 적용할 수 있다.
