# **44장 REST API**

- [**44장 REST API**](#44장-rest-api)
  - [**들어가며 🎈**](#들어가며-)
  - [**REST API의 구성**](#rest-api의-구성)
  - [**REST API 설계 원칙**](#rest-api-설계-원칙)
  - [**JSOn Server를 이용한 REST API 실습**](#json-server를-이용한-rest-api-실습)
    - [**JSON Server 설치**](#json-server-설치)
    - [**db.json 파일 생성**](#dbjson-파일-생성)
    - [**JSON Server 실행**](#json-server-실행)
    - [**GET 요청**](#get-요청)
    - [**POST 요청**](#post-요청)
    - [**PUT 요청**](#put-요청)
    - [**PATCH 요청**](#patch-요청)
    - [**DELETE 요청**](#delete-요청)

<br>

## **들어가며 🎈**

본 정리글은 이웅모강사님의 모던 자바스크립트 Deep Dive를 공부하고 습득하기 위해 제 나름대로 정리한 글 입니다. 보다 상세한 정보를 원하신다면 해당 서적을 참고하시길 바랍니다.

<br>

REST는 HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처고, REST API는 REST를 기반으로 서비스 API를 구현한 것을 의미한다.

<br>

## **REST API의 구성**

- REST API는 자원<sup>resource</sup>, 행위<sup>verb</sup>, 표현<sup>representation</sup>의 3가지 요소로 구성된다.
- REST는 자체 표현 구조<sup>self-descriptiveness</sup>로 구성되어 REST API만으로 HTTP 요청의 내용을 이해할 수 있다.

  <center>**구성 요소**<center> | <center>**내용**</center> | <center>**표현 방법**</center>
  --- | --- | ---
  자원 | 자원 | URI(엔드포인트)
  행위 | 자원에 대한 행위 | HTTP 요청 메서드
  표현 | 자원에 대한 행위의 구체적 내용 | 페이로드

<br>

---

## **REST API 설계 원칙**

REST에서 가장 중요한 기본적인 원칙은 두가지다.

1.  **URI는 리소스를 표현해야 한다.**

        URI는 리소스를 표현하는 데 중점을 두어야 한다. 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다. 따라서 이름에 `get` 같은 행위에 대한 표현이 들어가서는 안된다.

        ```bash
        # bad
        GET /getTodos/1
        GET /todos/show/1

        # good
        GET /todos/1
        ```

    <br>

2.  **리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.**

    HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다.
    <center>**HTTP 요청 메서드**<center> | <center>**종류**</center> | <center>**목적**<center> | **페이로드**
    --- | --- | --- | :---:
    GET | index/retrieve | 모든/특정 리소스 취득 | ❌
    POST | create | 리소스 생성 | ⭕
    PUT | replace | 리소스의 전체 교체 | ⭕
    PATCH | modify | 리소스 일부 수정 | ⭕
    DELETE | delete | 모든/특정 리소스 삭제 | ❌

    리소스에 대한 행위는 HTTP 요청 메서드를 통해 표현하며 URI에 표현하지 않는다.

    ```bash
    # bad
    GET /todos/delete/1

    # good
    DELETE /todos/1
    ```

<br>

---

## **JSOn Server를 이용한 REST API 실습**

### **JSON Server 설치**

가상 REST API 서버를 구축할 수 있는 툴

```bash
$ mkdir json-server-exam && cd json-server-exam
$ npm init -y
$ npm install json-server --save-dev
```

<br>

### **db.json 파일 생성**

루트 폴더에 해당 파일 생성

```json
{
  "todos": [
    {
      "id": 1,
      "content": "HTML",
      "completed": true
    },
    {
      "id": 2,
      "content": "CSS",
      "completed": false
    },
    {
      "id": 3,
      "content": "JavaScript",
      "completed": true
    }
  ]
}
```

<br>

### **JSON Server 실행**

```bash
## 파일의 변경을 감지하게 하려면 watch 옵션을 추가한다
$ json-server --watch db.json
```

```bash
## 포트를 변경하려면 port 옵션을 추가한다(기본 3000)
$ json-server --watch db.json --port 5000
```

위와 같이 매번 명령어를 입력하는 것은 번거로우니 `package.json` 파일의 script의 `"start"` 부분을 수정하자.

<br>

### **GET 요청**

todos 리소스에서 모든 todo를 취득한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스에서 모든 todo를 취득(index)
      xhr.open('GET', '/todos');

      // HTTP 요청 전송
      xhr.send();

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 20이면 정상적으로 응답된 상태다.
        if (xhr.status === 200) {
          document.querySelector('pre').innerHTML = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

todos 리소스에서 id를 사용하여 특정 todo를 취득한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      const xhr = new XMLHttpRequest();

      // tods 리소스에서 id를 사용하여 특정 todo를 취득(retrieve)
      xhr.open('GET', '/todos/1');

      xhr.send();

      xhr.onload = () => {
        if (xhr.status === 200) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

<br>

### **POST 요청**

todos 리소스에 새로운 todo를 생성한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스에 새로운 todo를 생성
      xhr.open('POST', '/todos');

      // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
      xhr.setRequestHeader('content-type', 'application/json');

      // HTTP 요청 전송
      // 새로운 todo를 생성하기 위해 페이로드를 서버에 전송해야 한다.
      xhr.send(JSON.stringify({ id: 4, content: 'Angular', completed: false }));

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200(OK) 또는 201(Created)이면 정상적으로 응답된 상태다.
        if (xhr.status === 200 || xhr.status === 201) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

<br>

### **PUT 요청**

특정 리소스 전체를 교체할 때 사용한다. 다음 예제에서는 todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체를 교체한다.

PUT 요청 시에는 `setRequestHeader`메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스에서 id로 todo를 특정하여 id를 제외한 리소스 전체를 교체
      xhr.open('PUT', '/todos/4');

      // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
      xhr.setRequestHeader('content-type', 'application/json');

      // HTTP 요청 전송
      // 리소스 전체를 교체하기 위해 페이로드를 서버에 전송해야 한다.
      xhr.send(JSON.stringify({ id: 4, content: 'React', completed: true }));

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
        if (xhr.status === 200) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

<br>

### **PATCH 요청**

PATCH는 특정 리소스의 일부를 수정할 때 사용한다. 다음 예제에서는 todos 리소스의 id로 todo를 특정하여 completed만 수정한다. PATCH 요청 시에는 `setRequestHeader`메서드를 사용하여 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정해야 한다.

```html
<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스의 id로 todo를 특정하여 completed만 수정
      xhr.open('PATCH', '/todos/4');

      // 요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정
      xhr.setRequestHeader('content-type', 'application/json');

      // HTTP 요청 전송
      // 리소스를 수정하기 위해 페이로드를 서버에 전송해야 한다.
      xhr.send(JSON.stringify({ completed: false }));

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
        if (xhr.status === 200) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```

<br>

### **DELETE 요청**

```html
<!DOCTYPE html>
<html>
  <body>
    <pre></pre>
    <script>
      // XMLHttpRequest 객체 생성
      const xhr = new XMLHttpRequest();

      // HTTP 요청 초기화
      // todos 리소스에서 id를 사용하여 todo를 삭제한다.
      xhr.open('DELETE', '/todos/4');

      // HTTP 요청 전송
      xhr.send();

      // load 이벤트는 요청이 성공적으로 완료된 경우 발생한다.
      xhr.onload = () => {
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태다.
        if (xhr.status === 200) {
          document.querySelector('pre').textContent = xhr.response;
        } else {
          console.error('Error', xhr.status, xhr.statusText);
        }
      };
    </script>
  </body>
</html>
```
