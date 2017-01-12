# 서블릿 초기화 매개변수

서블릿 초기화 매개변수란?

> 서블릿을 생성하고 초기화 할 때, 즉 init()을 호출할 때 서블릿 컨테이너가 전달하는 데이터. 보통 데이터베이스 연결 정보와 같은 정적인 데이터를 서블릿에 전달할 때 사용한다.

서블릿 초기화 매개변수 설정 방법

1. DD파일(web.xml) 서블릿 배치 정보에 설정
2. 어노테이션을 사용하여 서블릿 소스 코드에 설정

* 가능한 소스 코드에서 분리해서 외부 파일에 두는 것을 추천. 이는 외부 파일에 두면 변경하기가 쉽기 때문이다. 실무에서도 데이터베이스 정보와 같은 시스템 환경과 관련된 정보는 외부 파일에 두어 관리한다.

## DD파일에 서블릿 초기화 매개변수 설정

```xml
<!-- 서블릿 선언 -->
<servlet>
    <servlet-name>MemberUpdateServlet</servlet-name>
    <servlet-class>spms.servlets.MemberUpdateServlet</servlet-class>
    <init-param>
        <param-name>driver</param-name>
        <param-value>org.mariadb.jdbc.Driver</param-value>
    </init-param>
    <init-param>
        <param-name>url</param-name>
        <param-value>jdbc:mariadb://87.98.290.99:3306/testdb</param-value>
    </init-param>
    <init-param>
        <param-name>username</param-name>
        <param-value>test</param-value>
    </init-param>
    <init-param>
        <param-name>password</param-name>
        <param-value>1111M</param-value>
    </init-param>
</servlet>
  
<!-- 서블릿을 URL과 연결 -->
<servlet-mapping>
    <servlet-name>MemberUpdateServlet</servlet-name>
    <url-pattern>/member/update</url-pattern>
</servlet-mapping>
```

<init-param> 태그가 서블릿 초기화 매개변수를 설정하는 태그이다. servlet 태그의 자식 요소로 param-name과 param-value 태그로 매개변수의 값을 지정한다.

매개변수의 값을 여러 개 설정하고 싶으면 <init-param> 요소를 여러 개 작성하면 된다. 서블릿 초기화 매개변수들은 오직 그 매개변수를 선언한 서블릿에서만 사용할 수 있으며 다른 서블릿은 사용할 수 없다.

이처럼 소스 파일 밖에 DB 정보를 두면 나중에 DB 정보가 바뀌더라도 web.xml만 편집하면 되기 때문에 유지보수가 쉬워진다. 실무에서도 변경되기 쉬운 값들은 xml파일이나 프로퍼티 파일(.properties)에 두어 관리한다.

## 어노테이션으로 서블릿 초기화 매개변수 설정

```java
@WebServlet(
    urlPatterns = {"/member/update"},
    initParams = {
        @WebInitParam(name="driver", value="org.mariadb.jdbc.Driver"),
        @WebInitParam(name="url", value="jdbc:mariadb://87.98.290.99:3306/testdb"),
        @WebInitParam(name="username", value="test"),
        @WebInitParam(name="password", value="1111")
    }
)
```


## 실습 : 회원 정보 조회와 변경

- 소스 코드에 박혀있던 데이터베이스 연결 정보를 web.xml로 이동
- 회원의 상세 정보를 조회하고 값을 변경하는 서블릿 작성

### MemberListServlet에 회원 상세 정보를 볼 수 있는 링크 추가

```java
while(resultSet.next()){
    out.println(
        resultSet.getInt("MNO") + ", " +
        "<a href='update?no=" + resultSet.getInt("MNO") + "'>" +
        resultSet.getString("MNAME") + "</a>, " +
        resultSet.getString("EMAIL") + ", " +
        resultSet.getDate("CRE_DATE") + "<br>"
        );
}
```

상세 정보를 조회하려면 회원 정보가 필요하다. 링크에 'no' 매개변수를 포함시킨다.

> <a href='update?no=1'>홍길동</a>


### MemberUpdateServlet 작성

회원 상세 정보를 조회 및 수정할 수 있는 Servlet 작성

#### doGet

```java
@SuppressWarnings("serial")
public class MemberUpdateServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        
        try {
            
            Class.forName(this.getInitParameter("driver"));
            connection = DriverManager.getConnection(
                        this.getInitParameter("url"),
                        this.getInitParameter("username"),
                        this.getInitParameter("password")
                    );
            statement = connection.createStatement();
            resultSet = statement.executeQuery(
                        "SELECT MNO, EMAIL, MNAME, CRE_DATE FROM Members " +
                        "WHERE MNO = " + request.getParameter("no")
                    );
            resultSet.next();
            
            response.setContentType("text/html; charset=utf-8");
            PrintWriter out = response.getWriter();
            
            out.println("<html><head><title>회원정보</title></head>");
            out.println("<body><h1>회원정보</h1>");
            out.println("<form action='update' method='post'>");
            out.println("번호 : <input type='text' name='no' value='" + 
                        request.getParameter("no") + "' readonly><br>" );
            out.println("이름 : <input type='text' name='name' value='" + 
                    resultSet.getString("MNAME") + "'><br>" );
            out.println("이메일 : <input type='text' name='email' value='" + 
                    resultSet.getString("EMAIL") + "'><br>" );
            out.println("가입일 : " + resultSet.getDate("CRE_DATE") + "<br>" );
            out.println("<input type='submit' value='저장'>");
            out.println("<input type='button' value='취소' onclick='location.href=\"list\"'>");
            out.println("</form></body></html>");
            
        } catch (Exception e) {
            throw new ServletException(e);
        } finally {
            try {
                if(resultSet != null) resultSet.close();
                if(statement != null) statement.close();
                if(connection != null) connection.close();
            } catch (Exception e2) { }
        }
    }    
}
```

1. jdbc 드라이버 클래스, java.sql.Driver를 구현한 클래스를 로딩한다. Class.forName()은 인자값으로 클래스 이름을 넘기면 해당 클래스를 찾아 로딩한다. 클래스 이름은 반드시 패키지 이름을 포함해야 한다(QName). 서블릿 초기화 매개변수에서 jdbc 드라이버 클래스의 이름을 얻어와서 로딩.
2. getInitParameter()는 DD파일 또는 @WebServlet으로 부터 init-param의 매개 변수 값을 가져와 꺼내준다. 반환값은 문자열이다.
3. 데이터베이스 연결 : 초기화 매개변수 이용. 나중에 데이터베이스의 주소가 바뀐다거나 사용자 이름이나 암호가 바뀌더라도 소스 코드를 손댈 필요 없이 DD파일만 수정하면 된다.

```java
connection = DriverManager.getConnection(
            this.getInitParameter("url"),
            this.getInitParameter("username"),
            this.getInitParameter("password")
);
```

요청 매개변수로 넘어온 회원 정보를 가지고 회원 정보를 질의. 단 한명의 회원정보를 가지고 오기 때문에 next()를 한 번만 호출한다.

```java
statement = connection.createStatement();
resultSet = statement.executeQuery(
    "SELECT MNO, EMAIL, MNAME, CRE_DATE FROM Members " +
     "WHERE MNO = " + request.getParameter("no")
    );
resultSet.next();
```

#### doPost

```java
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        Connection connection = null;
        PreparedStatement statement = null;
        
        try {
            Class.forName(this.getInitParameter("driver"));
            connection = DriverManager.getConnection(
                        this.getInitParameter("url"),
                        this.getInitParameter("username"),
                        this.getInitParameter("password")
                    );
            statement = connection.prepareStatement(
                    "UPDATE Members SET EMAIL = ?, MNAME = ?, MOD_DATE = NOW()" +
                    " WHERE MNO = ?"
                    );
            statement.setString(1, request.getParameter("email"));
            statement.setString(2, request.getParameter("name"));
            statement.setInt(3, Integer.parseInt(request.getParameter("no")));
            statement.executeUpdate();
            response.sendRedirect("list");
        } catch (Exception e) {
            throw new ServletException(e);
        } finally {
            try {
                if(statement != null) statement.close();
                if(connection != null) connection.close();
            } catch (Exception e2){ }
        }
    }
```

### 전체 코드

```java
@SuppressWarnings("serial")
public class MemberUpdateServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        
        try {
            
            Class.forName(this.getInitParameter("driver"));
            connection = DriverManager.getConnection(
                        this.getInitParameter("url"),
                        this.getInitParameter("username"),
                        this.getInitParameter("password")
                    );
            statement = connection.createStatement();
            resultSet = statement.executeQuery(
                        "SELECT MNO, EMAIL, MNAME, CRE_DATE FROM Members " +
                        "WHERE MNO = " + request.getParameter("no")
                    );
            resultSet.next();
            
            response.setContentType("text/html; charset=utf-8");
            PrintWriter out = response.getWriter();
            
            out.println("<html><head><title>회원정보</title></head>");
            out.println("<body><h1>회원정보</h1>");
            out.println("<form action='update' method='post'>");
            out.println("번호 : <input type='text' name='no' value='" + 
                        request.getParameter("no") + "' readonly><br>" );
            out.println("이름 : <input type='text' name='name' value='" + 
                    resultSet.getString("MNAME") + "'><br>" );
            out.println("이메일 : <input type='text' name='email' value='" + 
                    resultSet.getString("EMAIL") + "'><br>" );
            out.println("가입일 : " + resultSet.getDate("CRE_DATE") + "<br>" );
            out.println("<input type='submit' value='저장'>");
            out.println("<input type='button' value='취소' onclick='location.href=\"list\"'>");
            out.println("</form></body></html>");
            
        } catch (Exception e) {
            throw new ServletException(e);
        } finally {
            try {
                if(resultSet != null) resultSet.close();
                if(statement != null) statement.close();
                if(connection != null) connection.close();
            } catch (Exception e2) { }
        }
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("utf-8");
        Connection connection = null;
        PreparedStatement statement = null;
        
        try {
            Class.forName(this.getInitParameter("driver"));
            connection = DriverManager.getConnection(
                        this.getInitParameter("url"),
                        this.getInitParameter("username"),
                        this.getInitParameter("password")
                    );
            statement = connection.prepareStatement(
                    "UPDATE Members SET EMAIL = ?, MNAME = ?, MOD_DATE = NOW()" +
                    " WHERE MNO = ?"
                    );
            statement.setString(1, request.getParameter("email"));
            statement.setString(2, request.getParameter("name"));
            statement.setInt(3, Integer.parseInt(request.getParameter("no")));
            statement.executeUpdate();
            response.sendRedirect("list");
        } catch (Exception e) {
            throw new ServletException(e);
        } finally {
            try {
                if(statement != null) statement.close();
                if(connection != null) connection.close();
            } catch (Exception e2){ }
        }
    }
    
}
```











