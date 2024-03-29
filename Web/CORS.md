# **CORS**(Cross-Origin Resource Sharing, 교차 출처 리소스 공유)

### **개요**

- CORS(Cross-Origin Resource Sharing)는 웹 애플리케이션에서 동일한 출처가 아닌 리소스에 접근하는 것을 제어하는 보안 메커니즘이다. 웹 브라우저에서는 보안 상의 이유로 스크립트에서 다른 도메인, 포트, 프로토콜에서 호스팅되는 리소스에 직접 액세스할 수 없도록 제한한다. 따라서, 동일 출처 정책(Same-Origin Policy)에 위배되는 상황에서는 추가적인 설정이 필요하다.(ex: 개발환경에서의 Proxy 우회, axios에서의 `{ withCredentials: true }` 등)

- CORS는 서버 측과 브라우저 간의 통신을 위해 사용되며, 다음과 같은 원칙에 따라 작동한다:

  1. 원천 요청(Origin Request): 웹 브라우저는 리소스에 대한 요청을 보낼 때 요청 헤더에 Origin을 포함하여 보낸다. Origin은 현재 웹 페이지의 출처를 나타내는 정보.

  2. 액세스 제어 응답(Access-Control Response): 서버는 받은 원천 요청을 검사하고, 해당 요청이 허용되는 출처인지 확인한다. 서버는 액세스 제어 응답 헤더인 Access-Control-Allow-Origin을 포함하여 응답합니다. 이를 통해 특정 출처의 요청을 허용하는지 거부하는지를 지정할 수 있다.

  3. 사전 요청과 실제 요청(Pre-flight and Actual Request): 웹 브라우저는 서버에 실제 요청을 보내기 전에 사전 요청(preflight request)을 보낼 수 있다. 사전 요청은 실제 요청의 전처리 단계로, 서버가 허용하는지 여부를 확인하는 용도.

- CORS는 HTTP 메서드에 대한 제한, 특정 헤더의 사용, 쿠키 및 인증 정보의 전달 등 다양한 설정을 지원한다. 또한, 서버 측에서는 원천 요청을 검사하고 허용하는 출처를 설정할 수 있다.

- CORS는 웹 애플리케이션에서 보안을 강화하고, 다른 도메인 간의 자원 공유를 안전하게 처리하는 데 도움이 된다. 서버 측에서 CORS 정책을 적절히 구성하고, 클라이언트 측에서는 CORS 에러를 처리하는 방법을 알아야 한다.

<br>

### **CORS 해결 방법 예**

1. 서버 측 설정: 서버는 응답에 CORS 관련 헤더를 포함하여 브라우저에게 허용된 출처를 알려준다. 일반적으로 서버는 Access-Control-Allow-Origin 헤더를 사용하여 허용할 도메인을 지정한다. 서버는 또한 다른 CORS 관련 헤더, 예를 들어 Access-Control-Allow-Methods, Access-Control-Allow-Headers, Access-Control-Allow-Credentials 등을 사용하여 브라우저에게 추가적인 제어 및 구성을 제공할 수 있다.

2. 브라우저에서 프리플라이트 요청: 브라우저는 실제 요청 전에 프리플라이트(preflight) 요청을 보낸다. 이는 실제 요청을 보내기 전에 서버가 특정 메서드와 헤더를 허용하는지 확인하는 OPTIONS 메서드 요청이다. 서버는 프리플라이트 요청에 대한 응답으로 CORS 헤더를 포함하여 브라우저에게 허용 여부를 알려준다.

3. 보안 관련 헤더: 서버는 CORS를 통해 액세스를 허용하는 경우, 보안과 관련된 추가적인 헤더를 설정할 수 있다. 예를 들어 Access-Control-Allow-Credentials 헤더를 true로 설정하면, 브라우저는 인증된 요청에 대해서만 CORS를 허용한다.
