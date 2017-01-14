# 서블릿에서 뷰 분리하기

MVC 아키텍처를 위한 첫 작업으로 기존의 서블릿으로부터 뷰(View) 역할을 분리. 클라이언트로부터 요청이 들어오면 서블릿은 데이터를 준비(모델 역할)하여 JSP에 전달(컨트롤러 역할)한다. JSP는 서블릿이 준비한 데이터를 가지고 웹 브라우저로 출력할 화면을 만든다.

### 값 객체(VO) = 데이터 수송 객체(DTO)

데이터베이스에서 가져온 정보를 JSP 페이지에 전달하려면 그 정보를 담을 객체가 필요하다. 이렇게 값을 담는 용도로 사용하는 객체를 값 객체(value object)라고 부른다.
값 객체는 계층 간 또는 객체 간에 데이터를 전달하는데 이용하므로 ‘데이터 수송 객체(data transfer object)’라고도 부른다. 또한, 값 객체는 업무영역(business domain)의 데이터를 표현하기 때문에 객체지향 분석 및 설계 분야에서는 ‘도메인 객체(domain object)’라고도 한다.

## View 분리 실습                                               

### VO 생성 - spms.vo 패키지에 Memeber.java 생성

```java
public class Member {

    protected int no;
    protected String name;
    protected String email;
    protected String password;
    protected Date createDate;
    protected Date modifiedDate;
    
    public int getNo() {
        return no;
    }
    
    public Member setNo(int no) {
        this.no = no;
        return this;
    }
    
    public String getName() {
        return name;
    }
    
    public Member setName(String name) {
        this.name = name;
        return this;
    }
    
    public String getEmail() {
        return email;
    }
    
    public Member setEmail(String email) {
        this.email = email;
        return this;
    }
    
    public String getPassword() {
        return password;
    }
    
    public Member setPassword(String password) {
        this.password = password;
        return this;
    }
    
    public Date getCreateDate() {
        return createDate;
    }
    
    public Member setCreateDate(Date createDate) {
        this.createDate = createDate;
        return this;
    }
    
    public Date getModifiedDate() {
        return modifiedDate;
    }
    
    public Member setModifiedDate(Date modifiedDate) {
        this.modifiedDate = modifiedDate;
        return this;
    }
}
```


값 객체(VO) = 데이터 수송 객체(DTO)

Member 클래스는 회원 목록 출력에 필요한 데이터 뿐만 아니라 등록이나 변경할 때 사용하는 데이터도 포함하고 있다. 보통 데이터베이스 테이블에 대응하여 값 객체를 정의한다.

setter메서드의 리턴값이 void가 아니라 Member인 이유는 셋터 메서드를 연속으로 호출하여 값을 할당할 수 있게 하기 위함이다. 즉, 다음과 같은 코드를 작성 할 수 있다.

```
new Member().setNo(1).setName(“홍길동”).setEmail(“hong@test.com”);
```

### 서블릿에서 뷰 관련 코드 제거

MemberListServlet 클래스에서 뷰 역할을 분리하기 위해 출력 코드를 제거한다. 데이터베이스에서 회원 정보를 가져와 Member 객체에 담고 Member 객체를 ArrayList에 추가한다. request에 회원 목록 데이터를 보관하여 JSP로 출력을 위임한다.

```java
@WebServlet("/member/list")
@SuppressWarnings("serial")
public class MemberListServlet extends HttpServlet {
    
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        
        ServletContext servletContext = this.getServletContext();
        
        try {
            Class.forName(servletContext.getInitParameter("driver"));
            connection = DriverManager.getConnection(
                    servletContext.getInitParameter("url"),
                    servletContext.getInitParameter("username"),
                    servletContext.getInitParameter("password")
                    );
            // connection 구현체를 이용해서 SQL문 실행 할 객체 준비
            statement = connection.createStatement();
            // SQL문 실행 결과를 resultSet형태로 반환하여 저장
            resultSet = statement.executeQuery(
                        "SELECT MNO, MNAME, EMAIL, CRE_DATE " +
                        "FROM Members ORDER BY MNO ASC"
                    );
            response.setContentType("text/html; charset=utf-8");
            ArrayList<Member> members = new ArrayList<Member>();
            
            //데이터베이스에서 회원 정보를 가져와 Member 객체에 담는다.
            //그리고 Member 객체를 ArrayList에 추가한다.
            while(resultSet.next()) {
                members.add(new Member()
                        .setNo(resultSet.getInt("MNO"))
                        .setName(resultSet.getString("MNAME"))
                        .setEmail(resultSet.getString("EMAIL"))
                        .setCreateDate(resultSet.getDate("CRE_DATE"))
                );  
            }
            
            //request에 회원 목록 데이터 보관
            request.setAttribute("members", members);
            
            //JSP로 출력을 위임한다.
            RequestDispatcher rd = request.getRequestDispatcher(
                        "/member/MemberList.jsp" );
            rd.include(request, response);
        
        } catch (Exception e) {
            throw new ServletException(e);
        } finally {
            try {
                // 자원 해제(역순으로 진행)
                if(resultSet != null) resultSet.close();
                if(statement != null) statement.close();
                if(connection != null) connection.close();
            } catch (Exception e2) { }
        }
    }
}
```

#### RequestDispatcher를 이용한 forward, include

이 객체는 HttpServletRequest를 통해 얻는다. 반드시 어떤 서블릿, 또는 JSP으로 위임할 것인지 알려줘야 한다.

RequestDispatcher 객체를 얻었으면 포워드(Forward)하거나 인클루드(include)하면 된다.
포워드로 위임하면 해당 서블릿으로 제어권이 넘어간 후 다시 돌아오지 않는다. 인클루드로 위임하면 해당 서블릿으로 제어권을 넘긴 후 그 서블릿이 작업을 끝내면 다시 제어권이 넘어온다.

#### request.setAttribute("members", members)

ServletRequest는 클라이언트의 요청을 다루는 기능 외에 어떤 값을 보관하는 기능도 있다. setAttribute()를 호출하여 값을 보관할 수 있고, getAttribute()를 호출하여 보관된 값을 꺼낼 수 있다. MemberListServlet의 request 객체는 MemberList.jsp와 공유하기 때문에, request에 값을 담아두면 MemberList.jsp에서 꺼내 쓸 수 있다.

### 뷰 컴포넌트 만들기

WebContent 아래에 member/MemberList.jsp 파일 생성

```jsp
<%@page import="spms.vo.Member"%>
<%@page import="java.util.ArrayList"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <h1>회원 목록</h1>
    <p><a href="add">신규회원</a></p>
    <%
        ArrayList<Member> members = (ArrayList<Member>)request.getAttribute("members");
        for(Member member : members) {
    %>
    <%=member.getNo() %>,
    <a href="update?no=<%=member.getNo()%>"><%=member.getName() %></a>,
    <%=member.getEmail() %>,
    <%=member.getCreateDate() %>
    <a href="delete?no=<%=member.getNo()%>">[삭제]</a><br>
    <%} %>
</body>
</html>
```

## 포워딩과 인클루딩 : 서블릿끼리 작업을 위임하는 방법

1. 포워드 방식 : 작업을 한 번 위임하면 다시 이전 서블릿으로 제어권이 돌아오지 않는다.
2. 인클루드 방식 : 다른 서블릿으로 작업을 위임한 후, 그 서블릿의 실행이 끝나면 다시 이전 서블릿으로 제어권이 넘어온다.

### 포워딩 실습

서블릿 실행 중 오류가 발생하면 예외 객체를 던지는 대신 포워딩으로 오류 정보를 출력하는 페이지로 위임

Error.html를 WebContent 아래에 생성

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>시스템 오류</title>
</head>
<body>
    <p>요청을 처리하는 중에 문제가 발생했습니다. 잠시 후에 다시 요청하시기 바랍니다.
    만약 계속해서 이 문제가 발생한다면 시스템 운영팀에 연락하기 바랍니다.</p>
</body>
</html>
```

MemberListServlet의 오류 처리 부분을 다음과 같이 수정한다.

```java
} catch (Exception e) {
            request.setAttribute("error", e);
            RequestDispatcher rd = request.getRequestDispatcher("/Error.html");
            rd.forward(request, response);
        }
```

예외를 던지는 코드를 제거하고, Error.html로 포워딩하는 코드를 추가한다. 당장은 쓰지 않지만 나중에 오류에 대한 상세 내용을 출력할 때 사용하기 위해 예외 객체를 request에 보관해 둔다.

### 인클루딩 실습

MemberList.jsp에서 인클루딩을 이용해서 상단 내용을 출력하는 Header.jsp와 하단 내용을 출력하는 Tail.jsp를 포함.

WebContent 아래에 Header.jsp와 Tail.jsp 생성.

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <div style="background-color:#00008b; color:#ffffff; height: 20px; padding: 5px;">
    SPMS(Simple Project Management System)
    </div>
</body>
</html>
```

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <div style="background-color:#f0fff0; height: 20px; padding: 5px; margin-top:10px;">
        SPMS &copy; 2016
    </div>
</body>
</html>
```

MemberList.jsp 파일에 <jsp:include> 태그 추가.

```html
<body>
    <jsp:include page="/Header.jsp" />
    <h1>회원 목록</h1>
    <p><a href="add">신규회원</a></p>

    <%
        ArrayList<Member> members = (ArrayList<Member>)request.getAttribute("members");
        for(Member member : members) {
    %>
    <%=member.getNo() %>,
    <a href="update?no=<%=member.getNo()%>"><%=member.getName() %></a>,
    <%=member.getEmail() %>,
    <%=member.getCreateDate() %>
    <a href="delete?no=<%=member.getNo()%>">[삭제]</a><br>
    <%} %>
    <jsp:include page="/Tail.jsp" />
</body>
```

JSP에서 포워딩이나 인클루딩은 <jsp:forward>와 <jsp:include>를 사용한다. 결국 이 태그들은 JSP 엔진에 의해 서블릿이 생성될 때 다음과 같은 코드로 바뀐다.

```java
RequestDispatcher rd = request.getRequestDispatcher("/Header.jsp");
rd.include(request, response);
```















