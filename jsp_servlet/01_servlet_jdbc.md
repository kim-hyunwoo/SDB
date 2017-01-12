# 서블릿과 JDBC

```
- 클라이언트의 요청을 GET과 POST 등으로 구분해서 처리하는 방법
- 리다이렉트, 리프레시를 다루는 방법
- 초기화 매개변수를 이용해서 설정 정보를 외부 파일에 두는 방법
- 서블릿에서 이를 참고하는 방법
- 서블릿 실행 전, 후에 필터를 끼우는 방법
```

> jdbc를 이용해서 DB에 CRUD하는 것은 엔터프라이즈 시스템의 기본이다.

## 데이터베이스에서 데이터 가져오기

> 서블릿의 주된 역할 : 클라이언트가 요청한 데이터를 다룸

데이터베이스를 사용하기 위한 두 가지

1. 데이터베이스에 요청을 전달하고 결과를 받을 도구 : jdbc
2. 데이터베이스에 명령을 내릴 때 사용할 언어 : SQL

> 서블릿 <-> jdbc Driver <-(SQL)-> DB

#### 실습

1\. 회원 테이블 생성

MNO를 키본키, 자동증가로 설정. EMAIL 컬럼은 중복되지 않도록 유니크 설정

```sql
CREATE TABLE Members (
  MNO INT NOT NULL AUTO_INCREMENT COMMENT '회원일련번호',
  EMAIL VARCHAR(45) NOT NULL COMMENT '이메일',
  PWD VARCHAR(100) NOT NULL COMMENT '암호',
  MNAME VARCHAR(50) NOT NULL COMMENT '이름',
  CRE_DATE DATETIME NOT NULL COMMENT '가입일',
  MOD_DATE DATETIME NOT NULL COMMENT '마지막암호변경일',
  PRIMARY KEY (MNO),
  UNIQUE INDEX EMAIL_UNIQUE (EMAIL ASC)
);
```

2\. Members 테이블에 회원 데이터 입력

```sql
INSERT INTO Members (EMAIL, PWD, MNAME, CRE_DATE, MOD_DATE) VALUES ("s1@test.com", "1111", "홍길동", NOW(), NOW());
INSERT INTO Members (EMAIL, PWD, MNAME, CRE_DATE, MOD_DATE) VALUES ("s2@test.com", "1111", "임꺽정", NOW(), NOW());
INSERT INTO Members (EMAIL, PWD, MNAME, CRE_DATE, MOD_DATE) VALUES ("s3@test.com", "1111", "일지매", NOW(), NOW());
INSERT INTO Members (EMAIL, PWD, MNAME, CRE_DATE, MOD_DATE) VALUES ("s4@test.com", "1111", "이몽룡", NOW(), NOW());
INSERT INTO Members (EMAIL, PWD, MNAME, CRE_DATE, MOD_DATE) VALUES ("s5@test.com", "1111", "성춘향", NOW(), NOW());
```

```sql
SELECT * FROM Members;
```

3\. jdbc 드라이버 준비

WEB-INF > lib > mariadb-java-client-1.5.5.jar. 그리고 BuildPath를 통해 라이브러리에 추가해주면 된다. Web App Libraries에서 확인 가능하다.

## jdbc

프로그램에서 SQL문을 실행하여 데이터를 관리하기 위한 Java API. 다양한 데이터베이스에 대해서 별도의 프로그램을 만들 필요 없이, 해당 데이터베이스의 JDBC를 이용하면 하나의 프로그램으로 데이터베이스를 관리할 수 있다.

- oracle용 jdbc 드라이버는 Oracle 설치 시 자동으로 설치된다 : Oracle 설치 경로/ lib 디렉토리에서 ojdbc6_g.jar 파일을 복사해서 Java 클래스 패스로 붙여 넣으면 된다.

- MariaDB용 jdbc 드라이버 : mariadb-java-client-1.5.5.jar 파일 다운로드 후에 /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre/lib/ext 디렉토리에 복사해서 붙여 넣기.

- 이클립스 > 환경설정 > Java > Installed JRE's > edit에 jar 파일 추가

#### 데이터베이스 연결 순서

1. jdbc 드라이버 로드(DriverManager) : 메모리에 드라이버가 로드 됨.
2. 데이터베이스 연결(Connection) : Connection 객체 생성
3. SQL문 실행(Statement) : 객체를 통해 SQL문이 실행됨.
4. 데이터베이스 연결 해제(ResultSet) : SQL문의 결과 값을 ResultSet 객체로 받음.

Statement 객체 살펴보기

Statement(interface)

- executeQuery() : SQL문 실행 후 여러개의 결과 값이 생기는 경우에 사용(SELECT)
- executeUpdate() : SQL문 실행 후 테이블의 내용만 변경되는 경우 사용(INSERT, DELETE, UPDATE)

- executeQuery() 실행 후 반환되는 레코드 셋(반환형이 Set) ; ResultSet으로 반환
    - next() : 다음 레코드로 이동
    - previous() : 이전 레코드로 이동
    - first() : 처음으로 이동
    - last() : 마지막으로 이동
    - get메서드(getString, getInt ~ )
    
- executeUpdate() : 반환형이 int.

## 회원 목록 조회 서블릿 생성

```java
@WebServlet("/member/list")
public class MemberListServlet extends GenericServlet{

    @Override
    public void service(ServletRequest request, ServletResponse response) throws ServletException, IOException {
        
        //jdbc 객체 주소를 보관할 참조 변수 선언
        Connection connection = null;
        Statement statement = null;
        ResultSet resultSet = null;
        
        String driver = "org.mariadb.jdbc.Driver";
        String url = "jdbc:mariadb://87.98.290.99:3306/testdb"; //URL
        String id = "test"; //DBMS 사용자 아이디
        String pw = "1111"; //DMBS 사용자 암호
        
        try {
            Class.forName(driver);
            // DriverManager.registerDriver(new org.mariadb.jdbc.Driver());
            // DB연결 connection 객체
            connection = DriverManager.getConnection(url, id, pw);

            // connection 구현체를 이용해서 SQL문 실행 할 객체 준비
            statement = connection.createStatement();

            // SQL문 실행 결과를 resultSet형태로 반환하여 저장
            resultSet = statement.executeQuery(
                        "SELECT MNO, MNAME, EMAIL, CRE_DATE " +
                        "FROM Members ORDER BY MNO ASC"
                    );
            response.setContentType("text/html; charset=utf-8");
            PrintWriter out = response.getWriter();
            out.println("<html><head><title>회원목록</title></head>");
            out.println("<body><h1>회원목록</h1>");
            out.println("<a href='add'>신규등록</a><br>");
            while(resultSet.next()){
                out.println(
                    resultSet.getInt("MNO") + ", " +
                    resultSet.getString("MNAME") + ", " +
                    resultSet.getString("EMAIL") + ", " +
                    resultSet.getDate("CRE_DATE") + "<br>"
                );
            }
            out.println("</body></html>");
        
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

## HttpServlet으로 GET 요청 다루기

절대 경로와 상대 경로

URL이 '/'로 시작하면, 절대경로이고 '/'없이 시작하면 상대경로이다.

#### 절대경로

절대 경로는 웹 서버 루트를 기준으로 계산. 만약 링크 URL이 /web04/member/add라면 실제 URL은 다음과 같다.

- 서버루트 : http://localhost:8080
- 최종경로 : http://localhost:8080/web04/member/add
- 절대경로 : /web04/member/add

* 절대 경로를 작성할 때 현재 웹 애플리케이션의 경로를 빠트리면 안된다. 절대 경로로 URL을 계산할 때 현재 컨텍스트 루트의 경로가 자동 계산되지 않기 때문이다. 절대 경로를 사용할 때 문제가 되는 것은 컨텍스트 루트의 이름을 바꾸면 절대 경로를 사용한 모든 웹 페이지의 링크를 바꿔야 한다. 그래서 실무에서는 절대 경로를 사용하지 않고 상대 경로를 사용해서 요청 URL을 작성한다.

#### 상대경로

상대 경로는 현재 경로를 기준으로 계산. 현재 경로 /member/list를 기준으로 <a> 링크의 상대 경로를 계산하면 다음과 같다.

- 현재경로 : http://localhost:8080/web04/member/list
- 최종경로 : http://localhost:8080/web04/member/add
- 상대경로 : add

### 회원경로 입력 폼 만들기

```java
@WebServlet("/member/add")
@SuppressWarnings("serial")
public class MemberAddServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html; charset=utf-8");
        PrintWriter out = response.getWriter();
        out.println("<html><head><title>회원 등록</title></head>");
        out.println("<body><h1>회원 등록</h1>");
        out.println("<form action='add' method='post'>");
        out.println("이름 : <input type='text' name='name'><br>");
        out.println("이메일 : <input type='text' name='email'><br>");
        out.println("암호 : <input type='password' name='password'><br>");
        out.println("<input type='submit' value='추가'>");
        out.println("<input type='reset' value='취소'>");
        out.println("</form>");
        out.println("</body></html>");
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        
        //매개 변수 값의 인코딩 형식을 지정. getParameter 이전에 와야 한다.
        //한글 깨짐 방지
        request.setCharacterEncoding("UTF-8");
        Connection connection = null;
        PreparedStatement statement = null;
        
        String driver = "org.mariadb.jdbc.Driver";
        String url = "jdbc:mariadb://87.98.290.99:3306/testdb";
        String id = "test";
        String pwd = "1111";
        
        try {
            Class.forName(driver);
            connection = DriverManager.getConnection(url, id, pwd);
            statement = connection.prepareStatement(
                        "INSERT INTO Members(EMAIL, PWD, MNAME, CRE_DATE, MOD_DATE)" +
                        " VALUES(?, ?, ?, NOW(), NOW())"
                    );
            statement.setString(1, request.getParameter("email"));
            statement.setString(2, request.getParameter("password"));
            statement.setString(3, request.getParameter("name"));
            statement.executeUpdate();
            
            response.setContentType("text/html; charset=utf-8");
            PrintWriter out = response.getWriter();
            out.println("<html><head><title>회원등록결과</title></head>");
            out.println("<body>");
            out.println("<p>등록 성공입니다!</p>");
            out.println("</body></html>");
            
        } catch (Exception e) {
            throw new ServletException(e);
        } finally {
            try {
                if(statement != null) statement.close();
                if(connection != null) connection.close();
            } catch (Exception e2) { }
        }
    }
}
```

#### Get 요청이 발생하는 경우

1. 웹 브라우저 주소창에 URL을 입력한 후 엔터를 누를 때
2. a 태그로 만들어진 링크를 누를 때
3. Form 태그의 method 속성 값이 get일 경우(default = get)

호출 순서

> /member/add -> 서블릿 컨테이너 -> service() -> 서블릿(get, post)

#### prepareStatement

prepareStatement는 반복적인 질의를 하거나, 입력 매개변수가 많은 경우에 유용하다. 특히 이미지와 같은 바이너리 데이터를 저장하거나 변경할 때는 prepareStatement만이 가능하다.

#### 입력 매개변수

SQL문에서 ? 문자로 표시된 입력항목을 말한다. SQL문을 실행하기 전에 setXXX() 메서드를 호출해서 설정한다. 

```java
statement = connection.prepareStatement(
            "INSERT INTO Members(EMAIL, PWD, MNAME, CRE_DATE, MOD_DATE)" +
            " VALUES(?, ?, ?, NOW(), NOW())"
            );
statement.setString(1, request.getParameter("email"));
statement.setString(2, request.getParameter("password"));
statement.setString(3, request.getParameter("name"));
statement.executeUpdate();
```




































