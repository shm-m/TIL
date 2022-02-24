## JSP로 웹 어플리케이션 구현

서블릿을 이용하면 동적으로 HTML을 만들 수 있지만 자바코드만으로 HTML을 완성하는 것은 번거로움.
그래서 HTML 문서에 필요할 경우에만 자바 코드를 적용해서 동적으로 사용할 수 있는 것이 나왔는데 그것이 템플릿 엔진이다.

- **JSP**(Java Server Page)
  HTML에 자바코드를 삽입하여 동적 웹페이지를 생성하는 웹어플리케이션 도구. JSP는 실행 시 서블릿으로 변환되어 WAS에서 필요한 기능을 수행한다.

### JSP 환경 설정

JSP를 사용하려면 라이브러리를 추가해야함. `bundle.gradle`에 아래 내용 추가

```java
implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
implementation 'javax.servlet:jstl'
```

_Gradle Refresh 까먹지 말기_

<br>

### 회원 등록 폼 JSP

> jsp의 경우 웹페이지 루트 폴더인 `webapp`에 생성

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="/jsp/members/save.jsp" method="post">
        username: <input type="text" name="username" />
        age: <input type="text" name="age" />
        <button type="submit">전송</button>
    </form>
</body>
</html>
```

`<%@ page contentType="text/html;charset=UTF-8" language="java" %>` : JSP 문서임을 명시

첫줄과 같은 별도의 JSP 태그를 제외하고는 HTML과 형식이 똑같음.

> 웹 브라우저로 실행 시 서블릿을 통해 별도의 url을 맵핑한 것이 아니기 때문에 주소 입력시에 `.jsp`까지 입력해줘야 함

<br>

---

### 회원 저장 JSP

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ page import="Hello.servlet.domain.member.MemberRepository" %>
<%@ page import="Hello.servlet.domain.member.Member" %>

<%
    MemberRepository memberRepository = MemberRepository.getInstance();

    System.out.println("MemberSaveServlet.service");
    String username = request.getParameter("username");
    int age = Integer.parseInt(request.getParameter("age"));

    Member member = new Member(username, age);
    memberRepository.save(member);
%>

<html>
<head>
    <title>Title</title>
</head>
<body>
성공
<ul>
    <li>id = <%=member.getId()%></li>
    <li>username = <%=member.getUsername()%></li>
    <li>age = <%=member.getAge()%></li>
</ul>
<a href ="/index.html">메인</a>
</body>
</html>
```

JSP는 내장 문법을 통해 자바코드를 그대로 다 사용 가능, 즉 **JSP 내에서도 비즈니스 로직 구현 가능.**

`<%@ page import="" %>` : 자바의 import 문과 동일.
`<% %>` : 자바 코드 입력
`<%= %>` : 자바 코드 출력

또한 JSP 문법 상 **request와 response가 자동으로 서블릿으로 변환**되어서 바로 사용 가능하다.

<br>

---

### 회원목록 JSP

```java
<%@ page import="Hello.servlet.domain.member.MemberRepository" %>
<%@ page import="Hello.servlet.domain.member.Member" %>
<%@ page import="java.util.List" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    MemberRepository memberRepository = MemberRepository.getInstance();
    List<Member> members = memberRepository.findAll();
%>
<html>
<head>
    <title>Title</title>
</head>
<body>
<table>
    <thead>
    <th>id</th>
    <th>username</th>
    <th>age</th>
    </thead>
    <tbody>
    <%
        for (Member member : members) {
            out.write(" <tr>");
            out.write(" <td>" + member.getId() + "</td>");
            out.write(" <td>" + member.getUsername() + "</td>");
            out.write(" <td>" + member.getAge() + "</td>");
            out.write(" </tr>");
        }
    %>
    </tbody>
    </table>
</body>
</html>

```

`out` 역시 JSP에서 제공하는 예약어로 `response.getWriter()`를 호출할 필요없이 바로 사용 가능.

JSP 내에서는 자바코드 출력 시에는 `out.write()` 대신에 `<%= %>` 태그를 사용해도 되지만 태그안에 태그를 중첩해서 사용할 수 없기 때문에 for문 안에서는 `out.write()`을 사용해야 함

<br>

---

### 서블릿과 JSP의 한계

서블릿으로 개발할 때는 HTML 코드를 사용하는 것이 번거로웠고, 이를 극복하고자 JSP만을 사용할 때는 중간에 JSP 태그를 통해 자바 코드를 삽입하여 번거로웠다.

또한 JSP의 경우 코드의 상위는 자바코드를 통한 비즈니스 로직이고, 아래는 결과를 보여주기 위한 뷰 영역으로 구분되어 있는데, 이러면 규모가 큰 프로젝트 진행 시 JSP가 너무 많은 역할을 하고 자바코드가 JSP에 노출되어 있는 형태로 유지보수가 어려워짐.

#### MVC 패턴의 탄생

따라서 비즈니스 로직을 수행하는 영역과 사용자에게 노출될 뷰 영역을 분리하여 개발할 필요성이 대두됨. 이것이 MVC 패턴의 등장 이유임.
