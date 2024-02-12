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


***
# 10. 연산
* 타임리프 연산은 자바와 다르지 않다. HTML안에서 사용하기 때문에 HTML 엔티티를 사용하는 부분만 조심하면된다.

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <ul>
      <li>산술 연산
        <ul>
          <li>10 + 2 = <span th:text="10 + 2"></span></li>
          <li>10 % 2 == 0 = <span th:text="10 % 2 == 0"></span></li>
        </ul>
      </li>
  
      <li>비교 연산
        <ul>
          <li>1 > 10 = <span th:text="1 &gt; 10"></span></li>
          <li>1 gt 10 = <span th:text="1 gt 10"></span></li>
          <li>1 >= 10 = <span th:text="1 >= 10"></span></li>
          <li>1 ge 10 = <span th:text="1 ge 10"></span></li>
          <li>1 == 10 = <span th:text="1 == 10"></span></li>
          <li>1 != 10 = <span th:text="1 != 10"></span></li>
        </ul>
      </li>
  
      <li>조건식
      <ul>
      <li>(10 % 2 == 0)? '짝수':'홀수' = <span th:text="(10 % 2 == 0)? '짝수':'홀수'"></span></li>
      </ul>
      </li>
  
      <li>Elvis 연산자
        <ul>
          <li>${data}?: '데이터가 없습니다.' = <span th:text="${data}?: '데이터가 없습니다.'"></span></li>
          <li>${nullData}?: '데이터가 없습니다.' = <span th:text="${nullData}?: '데이터가 없습니다.'"></span></li>
        </ul>
      </li>
  
      <li>No-Operation
        <ul>
          <li>${data}?: _ = <span th:text="${data}?: _">데이터가 없습니다.</span></li>
          <li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li>
        </ul>
      </li>
    </ul>
  </body>
</html>
```
* **비교연산**: HTML 엔티티를 사용해야 하는 부분을 주의하자,
  + `>` (gt), `<` (lt), `>=` (ge), `<=` (le), `!` (not), `==` (eq), `!=` (neq, ne)
* **조건식**: 자바의 조건식과 유사하다.
* **Elvis 연산자**: 조건식의 편의 버전
* **No-Operation**: `_` 인 경우 마치 타임리프가 실행되지 않는 것 처럼 동작한다. 이것을 잘 사용하면 HTML의 내용 그대로 활용할 수 있다. 마지막 예를 보면 데이터가 없습니다. 부분이 그대로 출력된다.

****
# 11. 속성 값 설정
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
  <h1>속성 설정</h1>
  <input type="text" name="mock" th:name="userA" />
  <h1>속성 추가</h1>
  - th:attrappend = <input type="text" class="text" th:attrappend="class=' large'" /><br/>
  - th:attrprepend = <input type="text" class="text" th:attrprepend="class='large'" /><br/>
  - th:classappend = <input type="text" class="text" th:classappend="large" /><br/>
  <h1>checked 처리</h1>
  - checked o <input type="checkbox" name="active" th:checked="true" /><br/>
  - checked x <input type="checkbox" name="active" th:checked="false" /><br/>
  - checked=false <input type="checkbox" name="active" checked="false" /><br/>
  </body>
</html>
```

##### 속성 설정
* th:*` 속성을 지정하면 타임리프는 기존 속성을 `th:*` 로 지정한 속성으로 대체한다. 기존 속성이 없다면 새로 만든다.
* 예) `<input type="text" name="mock" th:name="userA" />`
* ➡️ 타임리프 렌더링 후 `<input type="text" name="userA" />`

##### 속성 추가
* `th:attrappend` : 속성 값의 뒤에 값을 추가한다.
* `th:attrprepend` : 속성 값의 앞에 값을 추가한다.
* `th:classappend` : class 속성에 자연스럽게 추가한다.

##### checked 처리
* HTML에서는 `<input type="checkbox" name="active" checked="false" />` 이 경우에도 checked 속성이 있기 때문에 checked 처리가 되어버린다.
* HTML에서 `checked` 속성은 `checked` 속성의 값과 상관없이 `checked` 라는 속성만 있어도 체크가 된다. 이런 부분이 `true` , `false` 값을 주로 사용하는 개발자 입장에서는 불편하다.
* 타임리프의 `th:checked` 는 값이 `false` 인 경우 `checked` 속성 자체를 제거한다.
  + `<input type="checkbox" name="active" th:checked="false" />`
  + ➡️ 타임리프 렌더링 후: `<input type="checkbox" name="active" />`
 

****
# 10. 반복
* 타임리프에서 반복은 `th:each`를 사용한다.
* 추가로 반복에서 사용할 수 있는 여러 상태 값을 지원한다.

### 10.1. 반복 예시
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>기본 테이블</h1>
    <table border="1">
    <tr>
    <th>username</th>
    <th>age</th>
    </tr>
    <tr th:each="user : ${users}">
    <td th:text="${user.username}">username</td>
    <td th:text="${user.age}">0</td>
    </tr>
    </table>
    <h1>반복 상태 유지</h1>
    <table border="1">
      <tr>
        <th>count</th>
        <th>username</th>
        <th>age</th>
        <th>etc</th>
      </tr>
      <tr th:each="user, userStat : ${users}">
        <td th:text="${userStat.count}">username</td>
        <td th:text="${user.username}">username</td>
        <td th:text="${user.age}">0</td>
        <td>
          index = <span th:text="${userStat.index}"></span>
          count = <span th:text="${userStat.count}"></span>
          size = <span th:text="${userStat.size}"></span>
          even? = <span th:text="${userStat.even}"></span>
          odd? = <span th:text="${userStat.odd}"></span>
          first? = <span th:text="${userStat.first}"></span>
          last? = <span th:text="${userStat.last}"></span>
          current = <span th:text="${userStat.current}"></span>
        </td>
      </tr>
    </table>
  </body>
</html>
```

##### 반복 기능
* `<tr th:each="user : ${users}">`
  + 반복시 오른쪽 컬렉션( `${users}` )의 값을 하나씩 꺼내서 왼쪽 변수( `user` )에 담아서 태그를 반복 실행합니다.
  + `th:each` 는 `List` 뿐만 아니라 배열, `java.util.Iterable` , `java.util.Enumeration` 을 구현한 모든 객체를 반복에 사용할 수 있습니다. `Map` 도 사용할 수 있는데 이 경우 변수에 담기는 값은 `Map.Entry` 입니다.
 
##### 반복 상태 유지
* `<tr th:each="user, userStat : ${users}">`
* 반복의 두번째 파라미터를 설정해서 반복의 상태를 확인 할 수 있습니다.
* 두번째 파라미터는 생략 가능한데, 생략하면 지정한 변수명( `user` ) + `Stat` 가 됩니다.
* 여기서는 `user` + `Stat` = `userStat` 이므로 생략 가능합니다.

##### 반복 상태 유지 기능
* `index` : 0부터 시작하는 값
* `count` : 1부터 시작하는 값
* `size` : 전체 사이즈
* `even` , `odd` : 홀수, 짝수 여부( `boolean` )
* `first` , `last` :처음, 마지막 여부( `boolean` )
* `current` : 현재 객체

***
# 11. 조건부 평가
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>if, unless</h1>
    <table border="1">
      <tr>
        <th>count</th>
        <th>username</th>
        <th>age</th>
      </tr>
      <tr th:each="user, userStat : ${users}">
      <td th:text="${userStat.count}">1</td>
      <td th:text="${user.username}">username</td>
      <td>
      <span th:text="${user.age}">0</span>
      <span th:text="'미성년자'" th:if="${user.age lt 20}"></span>
      <span th:text="'미성년자'" th:unless="${user.age ge 20}"></span>
      </td>
      </tr>
    </table>
    <h1>switch</h1>
    <table border="1">
      <tr>
        <th>count</th>
        <th>username</th>
        <th>age</th>
      </tr>
      <tr th:each="user, userStat : ${users}">
        <td th:text="${userStat.count}">1</td>
        <td th:text="${user.username}">username</td>
        <td th:switch="${user.age}">
          <span th:case="10">10살</span>
          <span th:case="20">20살</span>
          <span th:case="*">기타</span>
        </td>
      </tr>
    </table>
  </body>
</html>
```
##### **if, unless**
* 타임리프는 해당 조건이 맞지 않으면 태그 자체를 렌더링하지 않는다.
* 만약 다음 조건이 `false` 인 경우 `<span>...<span>` 부분 자체가 렌더링 되지 않고 사라진다.
* `<span th:text="'미성년자'" th:if="${user.age lt 20}"></span>`
##### **switch**
* `*` 은 만족하는 조건이 없을 때 사용하는 디폴트이다.

***
# 12. 주석
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>예시</h1>
    <span th:text="${data}">html data</span>
    <h1>1. 표준 HTML 주석</h1>
    <!--
    <span th:text="${data}">html data</span>
    -->

    <h1>2. 타임리프 파서 주석</h1>
    <!--/* [[${data}]] */-->
    <!--/*-->
    <span th:text="${data}">html data</span>
    <!--*/-->

    <h1>3. 타임리프 프로토타입 주석</h1>
    <!--/*/
    <span th:text="${data}">html data</span>
    /*/-->
  </body>
</html>
```

1. 표준 HTML 주석
  + 자바스크립의 표준 HTML 주석은 타임리프가 렌더링 하지 않고, 그대로 남겨둔다.
2. 타임리프 파서 주석
  + 타임리프 파서 주석은 타임리프의 주석. 랜더링 부분에서 주석 부분을 제거해준다. 코드 검사시 주석부분이 보이지 않는다.
3. 타임리프 프로토타입 주석
  + HTML 파일을 웹브라우저에서 열었을 때는 랜더링하지 않는다.
  + 하지만 타임리프 랜더링을 거치면 정상 랜더링된다.


***
# 13. 블록
```html
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>

    <th:block th:each="user : ${users}">
      <div>
      사용자 이름1 <span th:text="${user.username}"></span>
      사용자 나이1 <span th:text="${user.age}"></span>
      </div>
      <div>
      요약 <span th:text="${user.username} + ' / ' + ${user.age}"></span>
      </div>
    </th:block>

  </body>
</html>
```
* 사용 예시
``` html
<div>
사용자 이름1 <span>userA</span>
사용자 나이1 <span>10</span>
</div>
<div>
요약 <span>userA / 10</span>
</div>

<div>
사용자 이름1 <span>userB</span>
사용자 나이1 <span>20</span>
</div>
<div>
요약 <span>userB / 20</span>
</div>

<div>
사용자 이름1 <span>userC</span>
사용자 나이1 <span>30</span>
</div>
<div>
요약 <span>userC / 30</span>
</div>
```
* 실행 결과
* `<th:block>` 은 렌더링시 제거된다.

***
# 14. 자바스크립트 인라인인
##### 사용예시
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <!-- 자바스크립트 인라인 사용 전 -->
    <script>
      var username = [[${user.username}]];
      var age = [[${user.age}]];

      //자바스크립트 내추럴 템플릿
      var username2 = /*[[${user.username}]]*/ "test username";

      //객체
      var user = [[${user}]];
    </script>

      <!-- 자바스크립트 인라인 사용 후 -->
    <script th:inline="javascript">
      var username = [[${user.username}]];
      var age = [[${user.age}]];

      //자바스크립트 내추럴 템플릿
      var username2 = /*[[${user.username}]]*/ "test username";

      //객체
      var user = [[${user}]];
    </script>
  </body>
</html>
```
##### 자바스크립트 인라인 사용전 - 결과
```html
<script>
var username = userA;
var age = 10;
//자바스크립트 내추럴 템플릿
var username2 = /*userA*/ "test username";
//객체
var user = BasicController.User(username=userA, age=10);
</script>
```
##### 자바스크립트 인라인 사용후 - 결과
```html
<script>
var username = "userA";
var age = 10;
//자바스크립트 내추럴 템플릿
var username2 = "userA";
//객체
var user = {"username":"userA","age":10};
</script>
```

### 14.1. 텍스트 랜더링
* `var username = [[${user.username}]];`
  + 인라인 사용 전 ➡️ `var username = userA;`
  + 인라인 사용 후 ➡️ `var username = "userA";`
* 인라인 사용 전 렌더링 결과를 보면 `userA` 라는 변수 이름이 그대로 남아있다. 결과적으로 userA가 변수명으로 사용되어서 자바스크립트 오류가 발생한다. 다음으로 나오는 숫자 age의 경우에는 `"` 가 필요 없기 때문에 정상 렌더링 된다.
* 인라인 사용 후 렌더링 결과를 보면 문자 타입인 경우 `"` 를 포함해준다. 추가로 자바스크립트에서 문제가 될 수 있는 문자가 포함되어 있으면 이스케이프 처리도 해준다. 예) `"` `\"`

### 14.2. 자바스크립트 내추럴 템플릿
* var username2 = `/*[[${user.username}]]*/ "test username";`
  + 인라인 사용 전 ➡️ `var username2 = /*userA*/ "test username";`
  + 인라인 사용 후 ➡️ `var username2 = "userA";`
* 인라인 사용 전 결과를 보면 정말 순수하게 그대로 해석을 해버렸다. 따라서 내추럴 템플릿 기능이 동작하지 않고, 심지어 렌더링 내용이 주석처리 되어 버린다.
* 인라인 사용 후 결과를 보면 주석 부분이 제거되고, 기대한 "userA"가 정확하게 적용된다.

### 14.3. 객체
* `var user = [[${user}]];`
  + 인라인 사용 전 ➡️ `var user = BasicController.User(username=userA, age=10);`
  + 인라인 사용 후 ➡️ `var user = {"username":"userA","age":10};`
* 인라인 사용 전은 객체의 toString()이 호출된 값이다.
* 인라인 사용 후는 객체를 JSON으로 변환해준다.

### 14.4. 자바스크립트 인라인 each
##### 자바스크립트 인라인 each 사용 예시
```html
<script th:inline="javascript">
  [# th:each="user, stat : ${users}"]
  var user[[${stat.count}]] = [[${user}]];
  [/]
</script>
```

##### 자바스크립트 인라인 each 결과
```html
<script>
  var user1 = {"username":"userA","age":10};
  var user2 = {"username":"userB","age":20};
  var user3 = {"username":"userC","age":30};
</script>
```

***
# 15. 템플릿 조각
##### 템플릿 조각 사용예시
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <body>
    <footer th:fragment="copy">
    푸터 자리 입니다.
    </footer>
    <footer th:fragment="copyParam (param1, param2)">
    <p>파라미터 자리 입니다.</p>
    <p th:text="${param1}"></p>
    <p th:text="${param2}"></p>
    </footer>
  </body>
</html>
```
* fragment 코드
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1>부분 포함</h1>
    <h2>부분 포함 insert</h2>
    <div th:insert="~{template/fragment/footer :: copy}"></div>

    <h2>부분 포함 replace</h2>
    <div th:replace="~{template/fragment/footer :: copy}"></div>

    <h2>부분 포함 단순 표현식</h2>
    <div th:replace="template/fragment/footer :: copy"></div>

    <h1>파라미터 사용</h1>
    <div th:replace="~{template/fragment/footer :: copyParam ('데이터1', '데이터2')}"></div>
  </body>
</html>
```
* 부분 포함 insert : `th:insert` 를 사용하면 현재 태그( `div` ) 내부에 추가한다.
* 부분 포함 replace : `th:replace` 를 사용하면 현재 태그( `div` )를 대체한다.
* 부분 포함 단순 표현식 : `~{...}` 를 사용하는 것이 원칙이지만 템플릿 조각을 사용하는 코드가 단순하면 이 부분을 생략할 수 있다.
* 파라미터 사용 : 파라미터를 전달해서 동적으로 사용 가능

***
# 14.1 템플릿 레이아웃1
##### base.html
```html
<html xmlns:th="http://www.thymeleaf.org">
<head th:fragment="common_header(title,links)">
  <title th:replace="${title}">레이아웃 타이틀</title>

  <!-- 공통 -->
  <link rel="stylesheet" type="text/css" media="all" th:href="@{/css/
  awesomeapp.css}">
  <link rel="shortcut icon" th:href="@{/images/favicon.ico}">
  <script type="text/javascript" th:src="@{/sh/scripts/codebase.js}"></script>

  <!-- 추가 -->
  <th:block th:replace="${links}" />
</head>
```
##### layoutMain.html
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head th:replace="template/layout/base :: common_header(~{::title},~{::link})">
  <title>메인 타이틀</title>
  <link rel="stylesheet" th:href="@{/css/bootstrap.min.css}">
  <link rel="stylesheet" th:href="@{/themes/smoothness/jquery-ui.css}">
</head>
<body>
메인 컨텐츠
</body>
</html>
```

* `common_header(~{::title},~{::link})` 이 부분이 핵심이다.
  + `::title` 은 현재 페이지의 title 태그들을 전달한다.
  + `::link` 는 현재 페이지의 link 태그들을 전달한다.

* 메인 타이틀이 전달한 부분으로 교체되었다.
* 공통 부분은 그대로 유지되고, 추가 부분에 전달한 `<link>` 들이 포함된 것을 확인할 수 있다.
* 이 방식은 사실 앞서 배운 코드 조각을 조금 더 적극적으로 사용하는 방식이다. 쉽게 이야기해서 레이아웃 개념을 두고, 그 레이아웃에 필요한 코드 조각을 전달해서 완성하는 것으로 이해하면 된다.

***
# 16. 템플릿 레이아웃2
##### layout
```html
<!DOCTYPE html>
<html th:fragment="layout (title, content)" xmlns:th="http://www.thymeleaf.org">
<head>
  <title th:replace="${title}">레이아웃 타이틀</title>
</head>
<body>
<h1>레이아웃 H1</h1>
<div th:replace="${content}">
  <p>레이아웃 컨텐츠</p>
</div>
<footer>
레이아웃 푸터
</footer>
</body>
</html>
```

##### extend layout
``` html
<!DOCTYPE html>
<html th:replace="~{template/layoutExtend/layoutFile :: layout(~{::title},~{::section})}"xmlns:th="http://www.thymeleaf.org">
<head>
  <title>메인 페이지 타이틀</title>
</head>
<body>
<section>
  <p>메인 페이지 컨텐츠</p>
  <div>메인 페이지 포함 내용</div>
</section>
</body>
</html>
```

* `layoutFile.html` 을 보면 기본 레이아웃을 가지고 있는데, `<html>` 에 `th:fragment` 속성이 정의되어 있다. 이 레이아웃 파일을 기본으로 하고 여기에 필요한 내용을 전달해서 부분부분 변경하는 것으로 이해하면 된다.
* `layoutExtendMain.html` 는 현재 페이지인데, `<html>` 자체를 `th:replace` 를 사용해서 변경하는 것을 확인할 수 있다. 결국 `layoutFile.html` 에 필요한 내용을 전달하면서 `<html>` 자체를 `layoutFile.html` 로 변경한다.




