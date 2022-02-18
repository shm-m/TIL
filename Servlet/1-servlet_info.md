## 서블릿 (Servlet)

### 서블릿이란

- 클라이언트의 요청에 대해 동적으로 작동하는 웹 어플리케이션 컴포넌트
- 자바 쓰레드를 통해 동작
- HTTP 프로토콜서비스를 지원하는 HttpServlet을 상속 받는다
- MVC 패턴에서는 컨트롤러로 이용된다

<br>

### 서블릿 컨테이너

- 웹컨테이너라고도 불리며, 웹서버의 컴포넌트 중 하나
- 서블릿 컨테이너는 URL과 특정 서블릿을 맵핑하며, 서블릿 객체를 생성, 초기화, 호출, 종료하는 생명주기를 관리
- WAS는 클라이언트와 서버 간의 소켓 통신에 필요한 TCP/IP 연결관리와 HTTP 프로토콜 해석 등의 네트워크 기반 작업을
  **추상화** 함으로써 일종의 실행환경을 제공한다
- 즉 클라이언트로부터 요청이 오면 WAS에서 Request, Response 객체를 새로 만들어 서블릿 컨테이너에 있는 서블릿 객체를 호출
- 개발자는 Request 객체에서 제공하는 HTTP 요청 정보를 편리하게 꺼내서 사용하고, 비즈니스 로직을 구현한뒤, Response 객체에 응답 정보를 입력하면 된다. 이후 Response 객체에 담겨있는 내용으로 WAS가 HTTP 응답 정보를 생성한다.
- 이런 실행 환경에서 개발자는 WAS에서 제공하는 요청 & 응답 개념 위에서 비즈니스 로직에 집중하면 됨

![web_application_req_res_architecture](/assets/Servlet/servlet_info_1.png)

###### 출처 : 인프런 김영한 Spring MVC 1

<br>

### 서블렛 객체는 싱글톤으로 관리한다.

- 요청 때마다 생성하는 것이 비효율적이기 때문. 따라서 최초 로딩 시 객체를 미리 만들어 재활용하기에 모든 요청은
  동일한 서블릿 객체 인스턴스에 접근한다. 단, 이때 공유 변수 사용에 주의해야 한다. 해당 서블릿은 서블릿 컨테이너 종료 시 함께 종료된다
- JSP 역시 서블릿으로 변환되어 사용
- 이러한 서블릿은 동시 요청을 위해 멀티 쓰레드를 통해 처리 된다

<br>

---

### 서블릿 환경 설정

`@ServletComponentScan` : 자동으로 해당 패키지(하위 폴더 포함)에 있는 서블릿을 찾아 등록해줌

어플리케이션 클래스에 해당 어노테이션 추가

<br>

### 서블릿을 활용한 간단한 요청 & 응답

```java
@WebServlet(name = "helloServlet", urlPatterns = "/hello")
public class HelloServlet extends HttpServlet { // HttpServlet 상속

    // ctrl + o -> service(protected) 메소드 호출
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("HelloServlet.service"); //soutm
        System.out.println("request = " + request); //soutv
        System.out.println("response = " + response);

        String username =  request.getParameter("username");
        //URL에 있는 쿼리파라미터를 조회 - http://localhost:8080/hello?username=world

        System.out.println("username = " + username);

        //응답 메세지에 데이터 담기 - 개발자도구로 확인
        //header에 들어갈 부분들
        response.setContentType("text/plain");
        response.setCharacterEncoding("utf-8");

        //body에 들어갈 부분
        response.getWriter().write("hello " + username);

    }
}
```

`@WebServlet` : 서블릿임을 명시하는 어노테이션

`name` : 서블릿 이름

`urlPatterns` : 서블릿 맵핑, 이를 통해 WAS가 사용자가 입력한 URL에 해당하는 서블릿을 호출함

result)

```java
HelloServlet.service
request = org.apache.catalina.connector.RequestFacade@5e4e72
response = org.apache.catalina.connector.ResponseFacade@37d112b6
username = world
```

<br>

#### response.getWriter()

PrintWriter 타입이며 응답 스트림에 텍스트를 내보내기 위해서 사용

보통 `PrintWriter out = response.getWriter()`의 형식으로 사용하며 jsp 기본 예약어로도 사용 가능

<br>
