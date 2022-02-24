## HTTP 요청 데이터

HTTP 요청 메시지를 통해 클라이언트 -> 서버로 데이터를 전달하는 방법에는 3가지가 있다

1. GET - 쿼리 파라미터
2. POST - HTML From
3. API 메시지 바디 - HTTP message body에 데이터를 직접 담아서 요청

- HTTP API에서 주로 사용하며 단순 텍스트. JSON, XML 방식 존재
- 최근에는 주로 JSON 형식을 많이 사용하며 POST, PUT, PATCH 메소드 이용

<br>

---

### 1. GET 쿼리 파라미터

메시지 바디 없이 URL의 쿼리 파라미터에 데이터를 입력해서 전달.

주로 검색, 필터, 페이징 등에 이용

`ex) /url?username=test&age=10`

쿼리 파라미터는 위와 같이 URL에 `?`을 시작으로 보낼 수 있음. 추가 파라미터는 `&`로 구분

<br>

쿼리 파라미터로 제공되는 요청 데이터의 경우 HttpServletRequest로 다음과 같이 조회 가능하다.

#### 쿼리 파라미터 조회 메서드

```java
String username = request.getParameter("username"); //단일 파라미터 조회
Enumeration<String> parameterNames = request.getParameterNames(); //파라미터 이름들 모두 조회
Map<String, String[]> parameterMap = request.getParameterMap(); //파라미터를 Map 으로 조회
String[] usernames = request.getParameterValues("username"); //복수 파라미터 조회
```

<br>

#### 전체 파리미터 조회

```java
request.getParameterNames().asIterator()
        .forEachRemaining(paramName -> System.out.println(paramName + "=" + request.getParameter(paramName)));
```

`paramName` : key
`request.geParameter(paramName)` : value

<br>

#### 단일 파라미터 조회

```java
String username = request.getParameter("username");
String age = request.getParameter("age");
```

<br>

#### 이름이 같은 복수 파라미터 조회

동일한 파라미터의 복수 개의 value를 입력 가능, 하지만 대부분 1:1로 보내지 복수개로 보내는 경우는 드뭄

보통 value 값이 복수 개일 경우 `request.getParameter()` 사용 시 첫번 쨰 값을 반환

ex)
`?username=hello&username=hello2&age=20`

```java
String[] usernames = request.getParameterValues("username");
    for (String name : usernames) {
        System.out.println("username = " + name);
    }
```

<br>

---

### 2. POST HTML Form

HTML의 Form 태그를 활용.

`content-type: application/x-www-form-urlencoded`

메시지 바디에 **쿼리 파라미터 형식**으로 전달, 회원가입, 상품 주문 등에 사용

_따라서 GET 쿼리 파라미터 방식과 호환_ -> **쿼리 파라미터의 조회 메소드를 그대로 사용 가능하다.**

클라이언트의 입장에서는 방식의 차이가 존재하지만, 서버 입장에서는 형식이 동일하므로 동시 사용 가능.

> 즉, HTML Form을 이용한 데이터 조회 역시 `request.getParameter`로 가능하다.

<br>

HTML 파일 생성

```javascript
<!DOCTYPE html>
<html>
<head>
 <meta charset="UTF-8">
 <title>Title</title>
</head>
<body>
<form action="/request-param" method="post">
 username: <input type="text" name="username" />
 age: <input type="text" name="age" />
 <button type="submit">전송</button>
</form>
</body>
</html>

```

해당 html을 실행하여 데이터를 입력한 후 전송 버튼을 클릭하면 `form` 태그에 입력된 url로 이동한다.

POST의 HTML Form을 전송하면 웹 브라우저는 아래와 같은 HTTP 메시지를 만든다.

```java
POST /save HTTP/1.1
Host: localhost:8081
Content-Type: application/x-www-form-urlencoded
username=kim&age=20 //message body
```

`Content-Type`의 경우 메시지 바디의 데이터 형식을 지정하기 때문에 GET 쿼리 파라미터로 전송 시에는 존재 X
하지만 POST HTML Form 형식으로 전달 시에는 메시지 바디에 데이터를 넣으므로 이를 지정해줘야 한다.

<br>

---

### 3-1. API 메시지 바디 - 단순 텍스트

단순 텍스트로 HTTP 메시지 바디에 데이터를 직접 전달할 경우 `InputStream`을 사용해 읽을 수 있다.

```java
ServletInputStream inputStream = request.getInputStream();
String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
System.out.println("messageBody = " + messageBody);
```

postman으로 전달 시 `Content-Type`을 `text/plain`으로 지정.

`inputStream`의 경우 byte 코드를 반환하기 때문에 `copyToString()` 메서드를 통해 String으로 변환해야한다.

이 때 `문자표(Charset)`을 지정해줘야 함 -> utf-8

<br>

---

### 3-2. API 메시지 바디 - JSON

#### JSON 형식 전송

```java
POST http://localhost:8080/request-body-json
content-type: application/json
message body: {"username": "hello", "age": 20}
```

result)

```java
결과: messageBody = {"username": "hello", "age": 20}
```

<br>

JSON 결과를 파싱해서 사용하려면 자바 객체가 필요하다. 이 때 `Jackson` 이나 `Gson`과 같은 **JSON 변환 라이브러리**가 필요.

스프링부트 MVC 사용시 기본적으로 `Jackson `라이브러리 제공 (`ObjectMapper`)

따라서 JSON 형식으로 파싱할 수 있는 객체(`helloData.class`)를 미리 생성한다.

```java
@Getter @Setter
public class HelloData {

    private String username;
    private int age;
}
```

롬복에서 제공하는 `@Getter`, `@Setter`를 통해 자동으로 추가됨

getter를 통해 jSON 데이터를 출력 가능한 형태로 변환 가능하다.

```java
private ObjectMapper objectMapper = new ObjectMapper();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        ServletInputStream inputStream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);
        System.out.println("messageBody = " + messageBody);

        HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);

        System.out.println("helloDate.username: " + helloData.getUsername());
        System.out.println("helloDate.age: " + helloData.getAge());

        response.getWriter().write("ok");

    }

```

`objectMapper` : Jackson을 통해 JSON 결과를 파싱할 객체

`readValue()` : messageBdoy에 담긴 JSON 타입의 데이터를 자바 객체로 변환

result)

```java
messageBody={"username": "hello", "age": 20}
data.username=hello
data.age=20
```

**단순텍스트와 달리 자바 객체로 변환하므로써 각각의 parameter의 value값을 호출할 수 있다.**

<br>
HTML Form을 통해 전달되는 데이터 역시 메시지 바디를 통해 전송되므로 HTTP API 방식을 통해 직접 읽을 수 있다.

하지만 파라미터 조회 방식이 더 편하기 떄문에 해당 방법을 더 많이 사용.

<br>
