# 서블릿 프로그래밍

## 서블릿 만들기

java.servlet.Servlet 인터페이스 : 서블릿 컨테이너가 서블릿에 대해 호출할 메서드 정의.

> Servlet 인터페이스 -> GenericSerlet 추상 클래스 -> HttpServlet

**서블릿 클래스는 반드시 Servlet 인터페이스를 구현해야 함**

```java
public class HelloWorld implements Servlet {

    @Override
    public void init(ServletConfig config) throws ServletException { }

    @Override
    public ServletConfig getServletConfig() {return null;}

    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {}

    @Override
    public String getServletInfo() {return null;}

    @Override
    public void destroy() { }
}
```

### 서블릿 배치 정보 작성(DD파일, 어노테이션)

1\. DD파일(web.xml)

```xml
<!-- 서블릿 선언 -->
<servlet>
    <servlet-name>Hello</servlet-name> <!-- 서블릿 명칭 -->
    <servlet-class>lesson03.servlets.HelloWorld</servlet-class> <!-- QName 서블릿 클래스명 -->
</servlet>
  
<!-- 서블릿을 URL과 연결 -->
<servlet-mapping>
    <servlet-name>Hello</servlet-name>
    <url-pattern>/Hello</url-pattern>
</servlet-mapping>
```

2\. 어노테이션 : Servlet 3.0 이상부터 가능

```java
import javax.servlet.annotation.WebServlet;

@WebServlet("/Hello")
```

#### WebServlet의 주요 속성

1. name : 서블릿의 이름을 설정, 기본 값은 빈 문자열
2. urlPattern : 서블릿의 URL 목록을 설정하는 속성
3. value : urlPattern과 같은 용도.

```java
@WebServlet(name = "서블릿이름")
@WebServlet(urlPattern="/calc")
@WebServlet(urlPattern={"/calc", "/calc.do"}) // 배열로 여러개 표기 가능
@WebServlet(value="/calc") //또는
@WebServlet("/calc") // 어노테이션 문법에서 속성이름이 value인 경우 속성명 생략 가능
//만약 value 속성 외에 다른 속성의 값도 함께 설정한다면 value 속성명 생략 불가
@WebServlet(name = "Calculator", value="/calc")
```

### 웰컴파일 설정 web.xml

디렉토리의 기본 웹 페이지. 설정한 순서대로 적용된다.

```xml
<welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
</welcome-file-list>
```

### 웹 애플리케이션 배치하기

1. 이클립스 자동 배치
2. 운영 서버에 배치하기 : 배치할 파일을 하나의 웹 아카이브 파일(.war)로 만들어서 배치 폴더에 복사
    1. .war파일을 톰캣 서버가 설치된 폴더 /webapps 폴더에 복사-붙여넣기.
    2. 톰캣 서버가 설치된 bin 폴더에 서버 시작 스크립트 파일이 있음.
    3. .startup.sh를 터미널에서 실행하면 톰캣서버가 실행되며, .war 파일명으로 폴더가 생성된다.
    4. 웹 브라우저에서 접속하면 정상적으로 실행되는 것을 알 수 있다.
    5. .shutdown.sh 스크립트를 실행하면 톰캣 서버가 종료된다.

## GenericSerlet

앞에서 보았듯 Servlet 인터페이스를 직접 구현하면, 인터페이스에 선언된 모든 메서드를 구현해야만 한다. 사실 구현해야하는 메서드 중에서 service() 메서드만이 반드시 구현해야 하는 메서드이다. 나머지 메서드는 상황에 따라 구현하지 않아도 상관없다. 이러한 점을 개선하기 위해 등장한 것이 GenericSerlet 추상 클래스이다.

```java
public class HelloWorld extends GenericServlet{

    @Override //추상 메서드인 service()만 구현하면 된다.
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        System.out.println("service() 호출됨");
    }
}
```

#### 실습

```java
@WebServlet("/hello")
public class HelloWorld extends GenericServlet{

    @Override
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        
        int a = Integer.parseInt(request.getParameter("a"));
        int b = Integer.parseInt(request.getParameter("b"));
        
        response.setContentType("text/plain; charset=utf-8");
        PrintWriter writer = response.getWriter();
        writer.println("a = " + a + ", " + "b = " + b + "의 계산결과입니다.");
        writer.println("a + b = " + (a + b));
        writer.println("a - b = " + (a - b));
        writer.println("a * b = " + (a * b));
        writer.println("a / b = " + (a / b));
    }
}
```

http://localhost:8080/Web04/hello?a=10&b=20을 브라우저에 입력.

```
servletRequest 객체 : 클라이언트의 요청 정보를 다룰 때 사용
- getParameter() : GET이나 POST 요청으로 들어온 매개변수 값을 꺼낼 때 사용.

servletResponse 객체 : 응답과 관련된 기능을 제공.
클라이언트에게 출력하는 데이터의 인코딩 타입 정의, 문자집합 지정,
출력 데이터를 임시 보관하는 버퍼의 크기를 조정하거나, 데이터를 출력하기 위해서
출력 스트림을 준비할 때 이 객체를 사용한다.
- setContentType() : 출력할 데이터의 인코딩 형식과 문자집합 지정.
클라이언트에게 출력할 데이터의 정보를 알려주어야 클라이언트는 그 형식에 맞춰 올바르게 출력 가능하다.
```

GET 요청의 경우 매개변수 값이 URI에 포함되기 때문에, setCharacterEncoding()으로는 문자 집합 지정이 불가능하다. 때문에 URI의 인코딩 형식을 설정해야 한다.

> Servers -> server.xml -> <Connector> 태그에 아래와 같이 URIEncoding 추가

```xml
<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" URIEncoding="UTF-8"/>
```

* 서블릿 컨테이너? 서블릿의 생성, 실행, 소멸까지 서블릿의 생명주기를 관리하는 프로그램(ex: 톰캣)






















