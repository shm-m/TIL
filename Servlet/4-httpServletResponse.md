## HttpServletResponse

HttpServletResponse의 경우 개발자가 해당 객체에 HTTP 응답 정보를 제공하고, 해당 객체에 담겨진 내용을 바탕으로
WAS에서 HTTP 응답 정보를 생성한다.

### HttpServletResponse의 역할

1. HTTP 응답 메시지 생성
   - HTTP 응답코드 지정
   - 헤더 생성
   - 바디 생성 (응답 데이터)
2. 편의 기능 제공
   - Content-Type
   - 쿠키
   - Redirect

<br>

---

### Response 객체 기본 사용법

#### 1. HTTP 응답 코드 지정

```java
response.setStatus(HttpServletResponse.SC_OK); // = response.setStatus(200);
```

상태 코드의 경우 번호를 입력해도 됨 그러나 서블릿에서 제공하는 용어 사용이 편리

<br>

#### 2. 헤더 생성

```java
response.setHeader("Content-Type", "text/plain;charset=UTF-8");
response.setHeader("Cache-Control", "no-cache, no-store, must-revalidate");
response.setHeader("Pragma", "no-cache");
response.setHeader("my-header", "hello"); / 헤더 내용을 다음과 같이 임의로 지정해줄 수 있음
```

`setHeader(이름, 값)` : response 메시지의 정보를 지정하는 함수

<br>

보안상의 문제와 같이 캐시를 사용하면 안되는 경우에는 다음과 같은 메소드를 사용하면 된다.

`"Cache-Control", "no-cache, no-store, must-revalidate")` : HTTP 1.1 용 캐시 무효화

`"Pragma", "no-cache"` : HTTP 1.0 용 캐시 무효화

<br>

#### 3-1. 편의 기능 - Content

content 설정의 경우 위에서 입력한대로 `setHeader()` 함수를 사용 가능하지만 직접적으로 content를 설정할 수 있는 메서드들 또한 존재한다. _쿠키, 리다이렉트 역시 마찬가지_

```java
// setHeader() 사용 시,
// response.setHeader("Content-Type", "text/plain;charset=utf-8");

response.setContentType("text/plain");
response.setCharacterEncoding("utf-8");
response.setContentLength(2);
```

`setContentLength()`의 경우 생략 시 response에서 자동으로 생성해준다.

<br>

#### 3-2. Cookie

설정 Set-Cookie: myCookie=good; Max-Age=600

```java
// response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");

Cookie cookie = new Cookie("myCookie", "good");
cookie.setMaxAge(600); //600초

response.addCookie(cookie);
```

<br>

#### 3-3. Redirect

설정 : Status Code 302, Location: /basic/hello-form.html

```java
// response.setStatus(HttpServletResponse.SC_FOUND); //302
// response.setHeader("Location", "/basic/hello-form.html");

response.sendRedirect("/basic/hello-form.html");
```

<img src="/assets/Servlet/redirect_status.PNG" width="40%" height="40%"/>

##### 서버 구동 시 상태코드 값

<br>

---

### 전체코드

```java
@WebServlet(name = "responseHeaderServlet", urlPatterns = "/response-header")
public class ResponseHeaderServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // [status-line]
        response.setStatus(HttpServletResponse.SC_OK); // = response.setStatus(200);

        // [response-headers]
        response.setHeader("Content-Type", "text/plain;charset=UTF-8");
        response.setHeader("Cache-Control", "no-cache, no-store, must-revalidate");
         // cache 무효화
        response.setHeader("Pragma", "no-cache");
         // 헤더 내용을 다음과 같이 임의로 지정해줄 수 있음
        response.setHeader("my-header", "hello");

        // [header의 편의 메서드]
        content(response);
        cookie(response);
        redirect(response);

        // [message body]
        PrintWriter writer = response.getWriter();
        writer.println("ok");
    }

    private void content(HttpServletResponse response) {
        //Content-Type: text/plain;charset=utf-8
        //Content-Length: 2

        //response.setHeader("Content-Type", "text/plain;charset=utf-8");
        response.setContentType("text/plain");
        response.setCharacterEncoding("utf-8");

        //response.setContentLength(2); //(생략시 자동 생성)
    }

    private void cookie(HttpServletResponse response) {
        // 설정 : Set-Cookie: myCookie=good; Max-Age=600;
        // response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");
        // 위의 방식대로 해도 되나 아래가 더 간편
        Cookie cookie = new Cookie("myCookie", "good");
        cookie.setMaxAge(600); //600초
        response.addCookie(cookie);
    }


    private void redirect(HttpServletResponse response) throws IOException {
        // 설정 : Status Code 302, Location: /basic/hello-form.html
        // response.setStatus(HttpServletResponse.SC_FOUND); //302
        // response.setHeader("Location", "/basic/hello-form.html");
        response.sendRedirect("/basic/hello-form.html");
    }
}
```
