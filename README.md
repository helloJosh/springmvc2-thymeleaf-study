﻿# 스프링 MVC2편 - thymeleaf
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



















