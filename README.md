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

* 여기서 `#request`는 `HttpServletRequest` 객체가 그대로 제공되기 때문에 데이터를 조회하려면 `request.getParameter("data")`와 같이 불편하게 접근해야한다.
* 이러한 점을 해결하기 위해 편의 객체도 제공된다.
  + HTTP 요청 파라미터 접근: `param` ( 예 : `${param.paramData}`)
  + HTTP 세션 접근: `session` ( 예 : `${session.sessionData}`)
  + 스프링 빈 접근: `@` ( 예 : `&{@helloBean.hello('Spring!')}`)


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
* 기본객체사용예제
* 편의객체사용예제


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

***
# 7. 유틸리티 객체와 날짜
### 7.1. 타임리프 유틸리티 객체들
* `#message`: 메시지, 국제화 처리
* `#uris`:URI 이스케이프 지원
* `#dates`: `java.util.Date` 서식 지원
* `#calendars`: `java.util.Calendar` 서식 지원
* `#temporals`: 자바8 날짜 서식 지원
* `#numbers`: 숫자 서식 지원
* `#strings`: 문자 관련 편의 기능
* `#objects`: 객체 관련 기능 제공
* `#bools`: boolean 관련 기능 제공
* `#arrays`: 배열 관련 기능 제공
* `#lists`,`#sets`,`#maps`: 컬렉션 관련 기능 제공
* `#ids`: 아이디 처리 관련 기능 제공

### 7.2. 자바8 날짜
* 타임리프 자바8 날짜 지원 라이브러리 : `thymeleaf-extras-java8time`
* 자바8 날짜용 유틸리티 객체 : `#temporals`

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>LocalDateTime</h1>
    <ul>
      <li>default = <span th:text="${localDateTime}"></span></li>
      <li>yyyy-MM-dd HH:mm:ss = <span th:text="${#temporals.format(localDateTime,'yyyy-MM-dd HH:mm:ss')}"></span></li>
    </ul>
    <h1>LocalDateTime - Utils</h1>
    <ul>
      <li>${#temporals.day(localDateTime)} = <span th:text="${#temporals.day(localDateTime)}"></span></li>
      <li>${#temporals.month(localDateTime)} = <span th:text="${#temporals.month(localDateTime)}"></span></li>
      <li>${#temporals.monthName(localDateTime)} = <span th:text="${#temporals.monthName(localDateTime)}"></span></li>
      <li>${#temporals.monthNameShort(localDateTime)} = <span th:text="${#temporals.monthNameShort(localDateTime)}"></span></li>
      <li>${#temporals.year(localDateTime)} = <span th:text="${#temporals.year(localDateTime)}"></span></li>
      <li>${#temporals.dayOfWeek(localDateTime)} = <span th:text="${#temporals.dayOfWeek(localDateTime)}"></span></li>
      <li>${#temporals.dayOfWeekName(localDateTime)} = <span th:text="${#temporals.dayOfWeekName(localDateTime)}"></span></li>
      <li>${#temporals.dayOfWeekNameShort(localDateTime)} = <span th:text="${#temporals.dayOfWeekNameShort(localDateTime)}"></span></li>
      <li>${#temporals.hour(localDateTime)} = <span th:text="${#temporals.hour(localDateTime)}"></span></li>
      <li>${#temporals.minute(localDateTime)} = <span th:text="${#temporals.minute(localDateTime)}"></span></li>
      <li>${#temporals.second(localDateTime)} = <span th:text="${#temporals.second(localDateTime)}"></span></li>
      <li>${#temporals.nanosecond(localDateTime)} = <span th:text="${#temporals.nanosecond(localDateTime)}"></span></li>
    </ul>
  </body>
</html>
```
* 사용 예시

***
# 8. URL 링크
* 타임리프에서 URL을 생성할 때는 `@{...}` 문법을 사용하면 된다.
```java
@GetMapping("/link")
public String link(Model model) {
  model.addAttribute("param1", "data1");
  model.addAttribute("param2", "data2");
  return "basic/link";
}
```
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>URL 링크</h1>
    <ul>
      <li><a th:href="@{/hello}">basic url</a></li>
      <li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>
      <li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
      <li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
    </ul>
  </body>
</html>
```
##### 단순한 URL
* `@{/hello}` -> `/hello`
##### 쿼리 파라미터
* `@{/hello(param1=${param1},param2=${param2})}`
  + -> `/hello?param1=data1&param2=data2`
  + `()`에 있는 부분은 쿼리 파라미터로 처리된다.
##### 경로 변수
* `@{/hello/{param1}/{param2}(param1=${param1},param2=${param2})}`
  + ➡️ `/hello/data1/data2`
  + URL 경로상에 변수가 있으면 `()` 부분은 경로 변수로 처리된다.
  + 위의 경우는 param1, param2가 앞에 경로에 있기 때문에 경로 변수로 처리되었다.
  + 쿼리 파라미터의 경우는 경로에 {param1}, {param2}가 없기 때문에 쿼리 파리미터로 처리된다.
##### 경로 변수 + 쿼리 파라미터
* `@{/hello/{param1}(param1=${param1}, param2=${param2})}`
  + ➡️ `/hello/data1/?param2=data2`
  + 위의 경우도 마찬가지로 앞의 경로에 {param1}이 경로에 있기 때문에 param1은 경로 변수, param2는 쿼리 파라미터로 처리된 것이다.
  + 경로 변수와 쿼리 파리미터를 함께 사용할 수 있다.
* 상대경로, 절대경로, 프로토콜 기준을 표현할 수도 있다
  + `/hello`: 절대경로
  + `hello`: 상대경로
 

***
# 9. 리터럴
* 리터럴 : 소스 코드상에서 고정된 값을 말하는 용어

### 9.1. 타임리프 리터럴
* 문자 : `'hello'`
* 숫자 : `10`
* 불린 : `true`,`false`
* null : `null`

### 9.2. 문자 리터럴
* 타임리프에서 문자 리터럴은 항상 `'`(작은 따옴표)로 감싸야 한다.
* `<span th:text="'hello'">`
* 공백 없이 이어진다면 하나의 의미있는 토큰으로 인지해 작은 따옴표를 생략할 수 있다.
* `<span th:text="hello">`
> 룰: `A-Z` , `a-z` , `0-9` , `[]` , `.` , `-` , `_`

##### 오류
* `<span th:text="hello world!"></span>`
* 문자 리터럴은 원칙상 `'` 로 감싸야 한다. 중간에 공백이 있어서 하나의 의미있는 토큰으로도 인식되지 않는다.

##### 수정
* `<span th:text="'hello world!'"></span>`
* 이렇게 `'` 로 감싸면 정상 동작한다.

### 9.3. 사용예시
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>리터럴</h1>
    <ul>
    <!--주의! 다음 주석을 풀면 예외가 발생함-->
    <!-- <li>"hello world!" = <span th:text="hello world!"></span></li>-->
      <li>'hello' + ' world!' = <span th:text="'hello' + ' world!'"></span></li>
      <li>'hello world!' = <span th:text="'hello world!'"></span></li>
      <li>'hello ' + ${data} = <span th:text="'hello ' + ${data}"></span></li>
      <li>리터럴 대체 |hello ${data}| = <span th:text="|hello ${data}|"></span></li>
    </ul>
  </body>
</html>
```
##### **리터럴 대체(Literal substitutions)**
* `<span th:text="|hello ${data}|">`
* 마지막의 리터럴 대체 문법을 사용하면 마치 템플릿을 사용하는 것 처럼 편리하다.





