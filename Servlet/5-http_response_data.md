## HTTP 응답 데이터

HTTP 응답 메시지(서버 -> 클라이언트)는 다음과 같은 방법으로 데이터를 전달한다.

1. 단순 텍스트
2. HTML 응답
3. HTTP API - MessageBody에 JSON 형태로 전달

<br>

---

### 1. 단순 텍스트

단순 텍스트의 경우 `getWriter()`메서드를 통해 전달 가능하다.

```java
PrintWriter writer = response.getWriter();
writer.println("ok"); // ok라는 텍스트 전송
```

<br>

---

### 2. HTML 응답

```java
// Content-Type: text/html;charset=utf-8
response.setContentType("text/html");
response.setCharacterEncoding("utf-8");

PrintWriter writer = response.getWriter();
writer.println("<html>");
writer.println("<body>");
writer.println(" <div>안녕?</div>");
writer.println("</body>");
writer.println("</html>");
```

응답 데이터를 HTML 형식으로 반환 시에는 단순 텍스트와 마찬가지로 `getWriter()` 메소드를 활용한다.
단 Content-Type을 **`text/html`로 지정해**, HTML 형식임을 명시해야 한다.

<img src="/assets/Servlet/response_html.PNG" width="40%" height="40%"/>

##### 페이지 소스 보기 시 정상적인 html 형식으로 출력

위의 이미지를 통해서 알 수 있듯이 별도의 html 파일 혹은 템플릿 엔진을 사용하지 않고도 `getWriter()` 메소드를 통해
HTML 형식이 사용 가능함을 알 수 있다.

<br>

---

### 3. HTTP API - JSON

앞서 요청 데이터를 전달하기 위해 JSON 타입의 데이터를 자바 객체로 변환했던 것처럼,

응답 메시지에서도 자바 객체를 JSON 형태로 변환하기 위해 `Jackson `라이브러리 제공 (`ObjectMapper`)를 사용한다.

```java
@WebServlet(name="responseJsonServlet", urlPatterns = ("/response-json"))
public class ResponseJsonServlet extends HttpServlet {

    private ObjectMapper objectMapper = new ObjectMapper();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("application/json");
        response.setCharacterEncoding("utf-8");

        HelloData helloData = new HelloData();
        helloData.setUsername("kim");
        helloData.setAge(20);

        String result = objectMapper.writeValueAsString(helloData);
        response.getWriter().write(result);
    }
}
```

JSON으로 데이터를 반환할 시에는 Content-type을 `application/json`으로 지정해야 한다. 해당 콘텐트 타입은 자동으로 utf-8 형식을 사용하므로 charset을 지정하지 않아도 된다.

`objectMapper.writeValueAsString(자바 객체)` : 자바 객체를 JSON 형태의 문자로 변환 -> {"username":"kim", "age":20}
