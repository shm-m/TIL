## HttpServletRequest

앞서 말했듯이 WAS는 클라이언트와 서버간의 통신에서 필요한 네트워크 기반의 작업들을 추상화해줌.

서블릿은 개발자가 HTTP 요청 메시지를 편리하게 사용할 수 있도록 개발자 대신 HTTP 요청 메시지를 파싱하고,
그 결과를 `HttpServletRequest` 객체에 담아서 제공.

<br>

---

### HttpServletRequest 기본 제공 기능

#### HTTP 요청 메시지

```
ex)
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: application/x-www-form-urlencoded
username=kim&age=20
```

요청 메시지의 경우 크롬 개발자 도구로도 확인 가능하지만 `application.properties`에 로그 설정을 추가하여 콘솔 창으로
확인 가능 (단 해당 기능 시 성능저하가 발생할 수 있으므로 개발 시에만 사용)

```
logging.level.org.apache.coyote.http11=debug
```

<br>

요청 메시지가 전송될 때 사용자는 해당 메시지를 직접 파싱하여 쓸 수도 있지만, HTTpServletRequest 객체를 사용하면 아래와 같은 정보를 개별적으로 편리하게 조회할 수 있음.

- START LINE

  - HTTP 메소드
  - URL
  - 쿼리 스트링
  - 스키마, 프로토콜

- 헤더

  - 헤더 조회

- 바디

  - form 파라미터 형식 조회
  - message body 데이터 직접 조회

- 추가 기능
  - 임시저장소 : 해당 HTTP 요청이 시작부터 끝날 때까지 유지되는 기능
    - 저장 : `request.setAttribute(name, value)`
    - 조회 : `request.getAttribute(name)`
  - 세션 관리 : `request.getSession(create: true)`

<br>

---

### 1. START LINE 정보

```java
private void printStartLine(HttpServletRequest request) {
    System.out.println("request.getMethod() = " + request.getMethod());
    System.out.println("request.getProtocal() = " + request.getProtocol());
    System.out.println("request.getScheme() = " + request.getScheme());
    System.out.println("request.getRequestURL() = " + request.getRequestURL());
    System.out.println("request.getRequestURI() = " + request.getRequestURI());
    System.out.println("request.getQueryString() = " + request.getQueryString());
    System.out.println("request.isSecure() = " + request.isSecure());
    }
```

result)

```java
request.getMethod() = GET
request.getProtocal() = HTTP/1.1
request.getScheme() = http
request.getRequestURL() = http://localhost:8081/request-header
request.getRequestURI() = /request-header
request.getQueryString() = null
request.isSecure() = false
```

- `getQueryString()` : 쿼리 파라미터 내용
- `isSecure()`: https 여부

<br>

---

### 2. HEADER 정보

#### 1. 모든 정보 조회

- **기존 방식(Old Way)**

```java
private void printHeaders(HttpServletRequest request) {
    Enumeration<String> headerNames = request.getHeaderNames();
        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();
            System.out.println(headerName + ": " + headerName);
        }
    }
```

`getHeadrNames()` : http request에 있는 모든 header 정보를 꺼내옴
`Enumeration` : 컬렉션 프레임워크가 만들어지기 이전에 사용된 Iterator의 구버전
`hasMoreElements()` : boolean 타입으로 읽어올 요소가 남아있는 지 확인.
`nextElement()` : 다음 요소를 읽어옴 = Iterator의 `next()`

<br>

- **현재 방식(Current Way)**

```java
private void printHeaders(HttpServletRequest request) {
    request.getHeaderNames().asIterator()
        .forEachRemaining(headerName -> System.out.println(headerName + ": " + headerName));
    }
```

`forEachRemaining()` : 모든 요소가 처리되거나 작업에서 예외가 발생할 때까지 나머지 각 요소에 대해 지정된 작업을 수행하는 함수

<br>

result)

```java
host: host
connection: connection
cache-control: cache-control
sec-ch-ua: sec-ch-ua
sec-ch-ua-mobile: sec-ch-ua-mobile
sec-ch-ua-platform: sec-ch-ua-platform
upgrade-insecure-requests: upgrade-insecure-requests
user-agent: user-agent
accept: accept
sec-fetch-site: sec-fetch-site
sec-fetch-mode: sec-fetch-mode
sec-fetch-user: sec-fetch-user
sec-fetch-dest: sec-fetch-dest
accept-encoding: accept-encoding
accept-language: accept-language
```

<br>

#### 2. 편리한 조회

- **HOST 편의 조회**

```java
System.out.println("request.getServerName() = " + request.getServerName());
System.out.println("request.getServerPort() = " + request.getServerPort());
```

result)

```java
request.getServerName() = localhost
request.getServerPort() = 8081
```

- **Accept-Language 편의 조회**

```java
  request.getLocales().asIterator()
  .forEachRemaining(locale -> System.out.println("locale = " +
  locale));
  System.out.println("request.getLocale() = " + request.getLocale());
```

`getLocales()` : 전체 호출
`getLocale()` : 최상단에 있는 언어만 호출

result)

```java
locale = ko_KR
locale = ko
locale = en_US
locale = en
request.getLocale() = ko_KR
```

- **cookie**

```java
if (request.getCookies() != null) {
    for (Cookie cookie : request.getCookies()) {
        System.out.println(cookie.getName() + ": " + cookie.getValue());
    }
}
```

cookie 역시 헤더에 담겨져 있는 정보

- **content**

```java
  System.out.println("request.getContentType() = " + request.getContentType());
  System.out.println("request.getContentLength() = " + request.getContentLength());
  System.out.println("request.getCharacterEncoding() = " + request.getCharacterEncoding());
```

result)

```java
request.getContentType() = null
request.getContentLength() = -1
request.getCharacterEncoding() = UTF-8
```

`Content-Type`의 경우 해당 URL의 전송 방식이 GET이라 null값이 뜨지만 post로 지정할 경우 조회 가능

<br>

---

### 3. 기타정보

기타 정보의 경우 HTTP 메시지 정보는 아님, _즉 로그 설정을 통해서 조회 불가능_

-**Remote 정보**

클라이언트 기본 정보

```java
System.out.println("request.getRemoteHost() = " + request.getRemoteHost());
System.out.println("request.getRemoteAddr() = " + request.getRemoteAddr());
System.out.println("request.getRemotePort() = " + request.getRemotePort());
```

-**Local 정보**

```java
System.out.println("request.getLocalName() = " + request.getLocalName());
System.out.println("request.getLocalAddr() = " +  request.getLocalAddr());
System.out.println("request.getLocalPort() = " + request.getLocalPort());
```

result)

```java
//Remote 정보
request.getRemoteHost() = 0:0:0:0:0:0:0:1
request.getRemoteAddr() = 0:0:0:0:0:0:0:1
request.getRemotePort() = 51996

//Local 정보
request.getLocalName() = 0:0:0:0:0:0:0:1
request.getLocalAddr() = 0:0:0:0:0:0:0:1
request.getLocalPort() = 8081

```

<br>

---

[4. 바디 조회](/Servlet/3-http_request_data.md)
