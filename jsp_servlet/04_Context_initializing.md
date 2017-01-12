# 콘텍스트 초기화 매개변수

서블릿 초기화 매개변수는 그 매개변수가 선언된 서블릿에서만 사용될 수 있다(다른 서블릿은 참조할 수 없다). 따라서 JDBC 드라이버와 데이터베이스 연결 정보에 대한 초기화 매개변수를 각 서블릿마다 별도로 설정해야 한다. 그러나 여러 서블릿이 사용하는 JDBC 드라이버와 DB 연결 정보가 같다면, 각각의 서블릿마다 초기화 매개변수를 선언하는 것은 번거롭고 낭비적인 작업이다. 이런 경우에 컨텍스트 초기화 매개변수를 사용한다. **컨텍스트 초기화 매개변수는 같은 웹 어플리케이션에 소속된 서블릿들이 공유하는 매개변수이다** .

### 콘텍스트 초기화 매개변수의 선언

DD파일(web.xml)

```xml
  <!-- 컨텍스트 매개변수 -->
  <context-param>
    <param-name>driver</param-name>
    <param-value>org.mariadb.jdbc.Driver</param-value>
  </context-param>
  <context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mariadb://87.98.290.99:3306/testdb<</param-value>
  </context-param>
  <context-param>
    <param-name>username</param-name>
    <param-value>test</param-value>
  </context-param>
  <context-param>
    <param-name>password</param-name>
    <param-value>1111</param-value>
  </context-param>
```

### 콘텍스트 초기화 매개변수의 사용

컨텍스트 초기화 매개변수의 값을 얻으려면 ServletContext 객체가 필요하다. HttpServlet으로부터 상속받은 getServletContext()를 호출해서 ServeltContext 객체를 준비한다.

```java
ServeltContext servletContext = this.getServletContext();
```

이 객체를 통해 getInitParameter()를 호출하면 web.xml에 선언된 컨텍스트 초기화 매개변수 값을 얻을 수 있다.

```java
Class.forName(servletContext.getInitParameter("driver"));
```

## MemberUpdateServlet에 적용

```java
@SuppressWarnings("serial")
@WebServlet("/member/update")
public class MemberUpdateServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        
        try {

            /* 서블릿 컨텍스트 사용 코드 */
            ServletContext servletContext = this.getServletContext();
            Class.forName(servletContext.getInitParameter("driver"));
            connection = DriverManager.getConnection(
                        servletContext.getInitParameter("url"),
                        servletContext.getInitParameter("username"),
                        servletContext.getInitParameter("password")
                    );
            /* 서블릿 컨텍스트 사용 */

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
            ServletContext servletContext = this.getServletContext();
            Class.forName(servletContext.getInitParameter("driver"));
            connection = DriverManager.getConnection(
                    servletContext.getInitParameter("url"),
                    servletContext.getInitParameter("username"),
                    servletContext.getInitParameter("password")
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

### 과제

1. MemberAddServlet과 MemberListServlet도 컨텍스트 초기화 매개변수를 사용하는 것으로 바꾸시오.
2. MemberListServlet의 슈퍼 클래스를 GenericServlet에서 HttpServlet으로 교체하시오.
3. 회원 정보를 삭제하는 서블릿을 만드시오.
4. 회원 목록 페이지에서 각 항목의 끝에 삭제 링크를 추가하시오. 링크를 클릭하면 회원 정보가 삭제되고, 회원 목록 페이지를 자동으로 갱신합니다.

#### 3번

1\. MemberUpdateServlet에서 회원정보 삭제 버튼 만들기

```java
out.println("<input type='button' value='삭제' " + 
            "onclick='location.href=\"delete?no=" + 
            request.getParameter("no") + "\"'>" );
```

2\. MemberDeleteServlet 만들기

```java
@WebServlet("/member/delete")
@SuppressWarnings("serial")
public class MemberDeleteServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Connection connection = null;
        PreparedStatement statement = null;
            
        try {
            ServletContext servletContext = this.getServletContext();
            Class.forName(servletContext.getInitParameter("driver"));
            connection = DriverManager.getConnection(
                        servletContext.getInitParameter("url"),
                        servletContext.getInitParameter("username"),
                        servletContext.getInitParameter("password")
                    );
            statement = connection.prepareStatement(
                        "DELETE FROM Members WHERE MNO = ?"
                    );
            statement.setInt(1, Integer.parseInt(request.getParameter("no")));
            statement.executeUpdate();
            response.sendRedirect("list");
            
        } catch (Exception e) {
            throw new ServletException(e);
        } finally {
            try {
                if (statement != null) statement.close();
                if (connection != null) connection.close();
            } catch (Exception e2) { }
        }
    }
}
```

#### 4번

MemberListServlet에 삭제 링크 추가

```java
while(resultSet.next()){
    out.println(
        resultSet.getInt("MNO") + ", " +
        "<a href='update?no=" + resultSet.getInt("MNO") + "'>" +
        resultSet.getString("MNAME") + "</a>, " +
        resultSet.getString("EMAIL") + ", " +
        resultSet.getDate("CRE_DATE") +
        "<a href='delete?no=" + resultSet.getInt("MNO") + "'> [삭제]</a><br>"
    );
}
```






