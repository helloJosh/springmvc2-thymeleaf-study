# 스프링 MVC2편 - thymeleaf
> 김영한 강사님 MVC 2편 thymeleaf 내용정리

***
# 1. 타임리프 소개
* 서버사이드 HTML 랜더링
  + 백엔드 서버에서 HTML을 동적으로 렌더링하는 용도로 사용
* 네츄럴 템플릿
  + 순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징
  + 랜더링된 화면을 정상적인 HTML 코드로 확인 가능

***
# 2. 타임리프 기본 기본 기능
```html
<html xmlns:th="http://www.thymeleaf.org">
```
* 타임리프 사용선언

### 2.1. 기본 표현식
```
간단한 표현
  - 변수 표현식: ${...}
  - 선택 변수 표현식: *{...}
  - 메시지 표현식: #{...}
  - 링크 URL 표현식: @{...}
  - 조각 표현식: ~{...}
리터럴
  - 텍스트: 'one text', 'Another one!'
  - 숫자: 0, 34, 3.0, 12.3
  - 불린: true, false
  - 널: null
  - 리터럴 토큰: one, sometext, main
문자 연산
  - 문자 합치기: +
  - 리터럴 대체: |The name is ${name}|
불린 연산
  - Binary operators: and, or
  - Boolean negation(unary operator): !, not
비교와 동등
  - 비교: >, <, >=, <= (gt, lt,ge,le)
  - 동등: ==, != (eq, ne)
조건 연산
  - If-then : (if) ? (then)
  - If-then-else: (if) ? (then) : (else)
  - Default: (value) ?: (defaultvalue)
특별한 토큰:
  - No-Operation: _
```

***
# 3. 텍스트 - text, utext
### 3.1. 사용법
* 태그 속성 사용법
  + `<span th:text="${data}">`
* 컨텐츠 안에서 직접 출력
  + `컨텐츠 안에서 직접 출력하기 = [[${data}]]`

### 3.2. 사용 예제
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
    <body>
    <h1>컨텐츠에 데이터 출력하기</h1>
    <ul>
      <li>th:text 사용 <span th:text="${data}"></span></li>
      <li>컨텐츠 안에서 직접 출력하기 = [[${data}]]</li>
    </ul>
    </body>
</html>
```
![image](https://github.com/helloJosh/springmvc2-thymeleaf-study/assets/37134368/76cbac29-555d-4781-bc9b-e1582b084b7b)

***
# 4. Escape & Unescape
### 4.1. Escape
* HTML은 `<`를 HTML 태그의 시작으로 인식하기 때문에 `<`를 태그의 시작이 아니라 문자로 표현할 수 있는 방법을 HTML 엔티디라고 한다.
* HTML에서 사용하는 특수 문자를 HTML 엔티티로 변경하는 것을 이스케이프(Escape)라고 한다.
  + `<` `&lt;`
  + `>` `&gt;`

### 4.2. Unescape
* Escape 기능을 사용하지 않는 기능을 Unescape라고한다.
* 타임리프는 다음 두 기능을 제공한다.
  + `th:text` -> `th:utext`
  + [[...]] -> [(...)}

### 4.3. 사용 예제
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>text vs utext</h1>
    <ul>
      <li>th:text = <span th:text="${data}"></span></li>
      <li>th:utext = <span th:utext="${data}"></span></li>
    </ul>
    <h1><span th:inline="none">[[...]] vs [(...)]</span></h1>
    <ul>
      <li><span th:inline="none">[[...]] = </span>[[${data}]]</li>
      <li><span th:inline="none">[(...)] = </span>[(${data})]</li>
    </ul>
  </body>
</html>
```
![image](https://github.com/helloJosh/springmvc2-thymeleaf-study/assets/37134368/9e2a904b-ae73-4cc3-b521-de686f0337c2)
* `th:inline="none"` : 타임리프는 `[[...]]` 를 해석하기 때문에, 화면에 `[[...]]` 글자를 보여줄 수 없다.

*** 
# 5. 변수 - SpringEl
### 5.1. 사용예제
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>SpringEL 표현식</h1>
    <ul>Object
      <li>${user.username} = <span th:text="${user.username}"></span></li>
      <li>${user['username']} = <span th:text="${user['username']}"></span></li>
      <li>${user.getUsername()} = <span th:text="${user.getUsername()}"></span></li>
    </ul>
    <ul>List
      <li>${users[0].username} = <span th:text="${users[0].username}"></span></li>
      <li>${users[0]['username']} = <span th:text="${users[0]['username']}"></span></li>
      <li>${users[0].getUsername()} = <span th:text="${users[0].getUsername()}"></span></li>
    </ul>
    <ul>Map
      <li>${userMap['userA'].username} = <span th:text="${userMap['userA'].username}"></span></li>
      <li>${userMap['userA']['username']} = <span th:text="${userMap['userA']['username']}"></span></li>
      <li>${userMap['userA'].getUsername()} = <span th:text="${userMap['userA'].getUsername()}"></span></li>
    </ul>
  </body>
</html>
```
### 5.2. Object
* `user.username` : user의 username을 프로퍼티(`user.getUsername()`) 접근
* `user['username']` : 위와 같음
* `user.getUsername()` : user의 `getUsername()`을 직접 호출

### 5.3. List
* `users[0].username` : List에서 첫 번쨰 회원을 찾고 username 프로퍼티 접근(`list.get(0).getUsername()`)
* `users[0]['username']` : 위와 같음
* `users[0].getUsername()` : List에서 첫 번째 회원을 찾고 메서드 직접 호출

### 5.4. Map
* `userMap['userA'].username`: Map에서 userA를 찾고, username 프로퍼티 접근(`map.get("userA").getUsername()`)
* `userMap['userA']['username']`: 위와 같음
* `userMap['userA'].getUsername()`: Map에서 userA를 찾고 메서드 직접 호출

### 5.5. 지역 변수 선언
```html
<h1>지역 변수 - (th:with)</h1>
<div th:with="first=${users[0]}">
  <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
</div>
```
* `th:with` 를 사용하면 지역 변수를 선언해서 사용할 수 있다.
* 지역 변수는 선언한 테그 안에서만 사용할 수 있다.

****
# 6. 기본 객체들

### 6.1. 타임리프 기본객체
* ${#request} - SpringBoot 3.0부터 제공하지 않는다.
* ${#response} - SpringBoot 3.0부터 제공하지 않는다.
* ${#session} - SpringBoot 3.0부터 제공하지 않는다.
* ${#servletContext} - SpringBoot 3.0부터 제공하지 않는다.
* ${#locale}
> SpringBoot 3.0에서 부터는 직접 model에 해당 객체를 추가해서 사용해야한다.

### 6.2. 스프링부트1.0~2.0(3.0미만)
```java
@GetMapping("/basic-objects")
public String basicObjects(HttpSession session) {
  session.setAttribute("sessionData", "Hello Session");
  return "basic/basic-objects";
}
@Component("helloBean")
static class HelloBean {
  public String hello(String data) {
    return "Hello " + data;
  }
}
```
* Controller 설정
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
    <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>식 기본 객체 (Expression Basic Objects)</h1>
    <ul>
      <li>request = <span th:text="${#request}"></span></li>
      <li>response = <span th:text="${#response}"></span></li>
      <li>session = <span th:text="${#session}"></span></li>
      <li>servletContext = <span th:text="${#servletContext}"></span></li>
      <li>locale = <span th:text="${#locale}"></span></li>
    </ul>
    <h1>편의 객체</h1>
    <ul>
      <li>Request Parameter = <span th:text="${param.paramData}"></span></li>
      <li>session = <span th:text="${session.sessionData}"></span></li>
      <li>spring bean = <span th:text="${@helloBean.hello('Spring!')}"></span></
    li>
    </ul>
  </body>
</html>
```
* 원래라면 `request.getParameter("data")` 처럼 불편하게 접근해야하지만
* 제공하는 기본객체인 `#request`를 사용하여 `HttpServletRequest` 객체를 제공받아 편하게 사용한다.

### 6.3. 스프링부트 3.0 이상
> 스프링부트 3.0 이상 부터는 이 기능이 Locale 제외하고는 사용할 수 없다.
```java
@GetMapping("/basic-objects")
public String basicObjects(Model model, HttpServletRequest request, HttpServletResponse response, HttpSession session) {
  session.setAttribute("sessionData", "Hello Session");
  model.addAttribute("request", request);
  model.addAttribute("response", response);
  model.addAttribute("servletContext", request.getServletContext());
  return "basic/basic-objects";
}
@Component("helloBean")
static class HelloBean {
  public String hello(String data) {
    return "Hello " + data;
  }
}
```
* 직접 request, response, session을 모델에 담아서 뷰에 넘겨서 사용한다.
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>식 기본 객체 (Expression Basic Objects)</h1>
    <ul>
      <li>request = <span th:text="${request}"></span></li>
      <li>response = <span th:text="${response}"></span></li>
      <li>session = <span th:text="${session}"></span></li>
      <li>servletContext = <span th:text="${servletContext}"></span></li>
      <li>locale = <span th:text="${#locale}"></span></li>
    </ul>
    <h1>편의 객체</h1>
    <ul>
      <li>Request Parameter = <span th:text="${param.paramData}"></span></li>
      <li>session = <span th:text="${session.sessionData}"></span></li>
      <li>spring bean = <span th:text="${@helloBean.hello('Spring!')}"></span></li>
    </ul>
  </body>
</html>
```
* 위 결과와 같은 결과가 나온다.













