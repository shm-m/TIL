## 서블릿으로 웹 어플리케이션 구현

MVC 패턴을 사용하기 이전에 서블릿만을 이용하여 간단한 회원 관리 웹 어플리케이션을 제작하여 서블릿의 장단점을 살펴본다.

<br>

---

### 회원 관리 웹 어플리케이션 요구사항

서블릿으로 기능을 구현하기에 앞서 회원 관리 프로그램을 만들기 위해선 회원 정보(이름, 나이)와 회원을 저장하고 저장된 회원을 조회하는 목록 기능이 필요하다.

#### 회원 도메인 모델

```java
@Getter
@Setter
public class Member {

    private Long id;
    private String username;
    private int age;

    // 기본 생성자
    public Member() {
    }

    public Member(String username, int age) {
        this.username = username;
        this.age= age;
    }
}
```

`id`의 경우 회원 저장소에서 자동으로 할당되도록 함

따라서 기본 생성자는 비워두고 `name`과 `age`만 할당하는 별도의 생성자를 정의한다.

<br>

#### 회원 저장소

```java
public class MemberRepository {

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    private static final MemberRepository instance = new MemberRepository();

    public static MemberRepository getInstance() {
        return instance;
    }

    private MemberRepository() {
    }

    public Member save(Member member) {
        member.setId(sequence++);
        store.put(member.getId(), member);
        return member;
    }

    public Member findById(Long id) {
        return store.get(id);
    }

    public List<Member> findAll() {
        return new ArrayList<>(store.values());
    }

    public void clearStore() {
        store.clear();
    }
}

```

`private static final MemberRepository instance = new MemberRepository();`

스프링의 경우 빈으로 등록하면 자동으로 싱글톤 패턴이 적용되지만 현재는 서블릿만을 이용해 작성하므로 싱글톤 등록이 필요하다.

`private MemberRepository() {}`

**싱글톤으로 생성 시에는 객체를 단 하나만 생성해 이를 공유**해아하므로 생성자를 `private` 접근자로 막아 인스턴스의 불필요한 다중 생성을 방지한다.

<br>

#### 회원 저장소 테스트 코드

```java
class MemberRepositoryTest {

    MemberRepository memberRepository = MemberRepository.getInstance();

    @AfterEach
    void afterEach() {
        memberRepository.clearStore();
    }

    @Test
    void save() {
        // given
        Member member = new Member("hello", 20);

        // when
        Member savedMember = memberRepository.save(member);

        // then
        Member findMember = memberRepository.findById(savedMember.getId());
        assertThat(findMember).isEqualTo(savedMember);
    }

    @Test
    void findAll() {
        // given
        Member member1 = new Member("member1", 20);
        Member member2 = new Member("member2", 21);

        memberRepository.save(member1);
        memberRepository.save(member2);

        // when
        List<Member> result = memberRepository.findAll();

        // then
        assertThat(result.size()).isEqualTo(2); // Assertions.assertThat()에서 단축키 alt+enter
        assertThat(result).contains(member1, member2);

    }
}
```

테스트 코드 작성 시에는 다음 테스트에 영향을 주지 않도록 `@AfterEach`를 통해 테스트가 진행될 때마다 `clearStore()`를 호출하여 초기화해야한다.

`asserThat()` : 실행한 결과가 제대로 이루어지는 지 검증 단계에서 사용하는 메서드

<br>

---

### 서블릿*만* 사용하여 회원 관리 웹 어플리케이션 구현

#### 회원 등록 HTML 폼

회원 등록 폼을 작성하려면 이전 [응답 데이터 작성]("/http_response_data.md") 시 사용했던 방법 활용

```java
@WebServlet(name ="memberFormServlet", urlPatterns = "/servlet/members/new-form")
public class MemberFormServlet extends HttpServlet {

    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");

        PrintWriter w = response.getWriter();
        w.write("<!DOCTYPE html>\n" +
                "<html>\n" +
                "<head>\n" +
                " <meta charset=\"UTF-8\">\n" +
                " <title>Title</title>\n" +
                "</head>\n" +
                "<body>\n" +
                "<form action=\"/servlet/members/save\" method=\"post\">\n" +
                " username: <input type=\"text\" name=\"username\" />\n" +
                " age: <input type=\"text\" name=\"age\" />\n" +
                " <button type=\"submit\">전송</button>\n" +
                "</form>\n" +
                "</body>\n" +
                "</html>\n");
    }
}
```

`private MemberRepository memberRepository = MemberRepository.getInstance();`

앞서 레포지토리를 싱글톤으로 작성했으므로 new 생성이 안됨

- 결과

<img src="/assets/Servlet/servlet_html_form.PNG" width="40%" />

<br>

#### 회원 저장

회원 저장 기능을 수행하기 위해 전송 버튼 클릭 시 이동하는 url로 맵핑되는 서블릿을 구현.

```java
@WebServlet(name ="memberSaveServlet", urlPatterns = "/servlet/members/save")
public class MemberSaveServlet extends HttpServlet {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("MemberSaveServlet.service");
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        PrintWriter writer = response.getWriter();
        writer.write("<html>\n" +
                "<head>\n" +
                " <meta charset=\"UTF-8\">\n" +
                "</head>\n" +
                "<body>\n" +
                "성공\n" +
                "<ul>\n" +
                " <li>id="+member.getId()+"</li>\n" +
                " <li>username="+member.getUsername()+"</li>\n" +
                " <li>age="+member.getAge()+"</li>\n" +
                "</ul>\n" +
                "<a href=\"/index.html\">메인</a>\n" +
                "</body>\n" +
                "</html>");
    }
}
```

회원 저장의 경우 클라이언트가 작성한 회원 이름 및 나이에 대한 요청 데이터를 받아와야 하므로 `getParameter()` 함수 사용

받아온 데이터를 각각 별도의 변수로 할당한뒤 해당 변수들을 `member` 객체의 파라미터로 저장, `save()` 메소드를 통해 레포지토리에 저장한다.

즉 순서를 정리하면 다음과 같다.

1. `getPrameter()`를 통해 요청 데이터를 조회하여 `Member` 객체를 생성
2. `Member` 객체를 `MemberRepository`에 저장
3. 해당 객체를 사용해 결과 화면용 HTML을 **동적**으로 만들어서 사용자에게 응답

<br>

#### 회원 목록 조회

```java
@WebServlet(name="memberListServlet", urlPatterns = "/servlet/members/")
public class MemberListServlet extends HttpServlet {
    MemberRepository memberRepository = MemberRepository.getInstance();
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        response.setContentType("text/html");
        response.setCharacterEncoding("utf-8");
        List<Member> members = memberRepository.findAll();
        PrintWriter w = response.getWriter();
        w.write("<html>");
        w.write("<head>");
        w.write(" <meta charset=\"UTF-8\">");
        w.write(" <title>Title</title>");
        w.write("</head>");
        w.write("<body>");
        w.write("<a href=\"/index.html\">메인</a>");
        w.write("<table>");
        w.write(" <thead>");
        w.write(" <th>id</th>");
        w.write(" <th>username</th>");
        w.write(" <th>age</th>");
        w.write(" </thead>");
        w.write(" <tbody>");
        for (Member member : members) {
            w.write(" <tr>");
            w.write(" <td>" + member.getId() + "</td>");
            w.write(" <td>" + member.getUsername() + "</td>");
            w.write(" <td>" + member.getAge() + "</td>");
            w.write(" </tr>");
        }
        w.write(" </tbody>");
        w.write("</table>");
        w.write("</body>");
        w.write("</html>");
    }
}

```

`memberRepository.findAll()`을 통해 모든 저장된 회원의 id를 불러와 배열에 저장한다. members의 경우 리스트 형태이기에 각각의 회원의 주소값만 반환하므로 for문을 통해서 개별 회원의 속성값을 출력해야한다.

- 결과

## <img src="/assets/Servlet/servlet_html_list.PNG" width="40%" />

---

<br>

이처럼 서블릿과 자바 코드만으로 정적인 HTML 문서를 동적으로 구현할 수 있었음.
그러나 지금까지의 방식처럼 HTML을 구현하기에는 각각의 태그마다 `getWriter()` 함수를 사용해야하므로 번거롭고 비효율적.

이 떄문에 JSP 혹은 타임리프와 같은 **템플릿 엔진**이 생김.

템플릿 엔진을 통해 차라리 자바코드로 HTML을 구현하기 보다 HTML 문서에 동적인 기능이 필요시 자바코드를 넣는게 편리할 수 있기 때문이다.
