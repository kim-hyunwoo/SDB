# JSP 액션 태그, EL, JSTL

## Jsp 액션 태그

JSP에서 기본으로 제공하는 태그들의 집합을 JSP 액션이라 한다.

```
1. <jsp:useBean>
자바 인스턴스를 준비. 보관소에서 자바 인스턴스를 꺼내거나, 자바 인스턴스를 새로 만들어 보관소에 저장하는 코드를 생성한다. 
자바 인스턴스를 자바 빈(Java Bean)이라고 부른다.

2. <jsp:setProperty>
자바 빈의 프로퍼티 값을 설정. 자바 객체의 셋터 메서드를 호출하는 코드를 생성.

3. <jsp:getProperty>
자바 빈의 프로퍼티 값을 꺼낸다. 자바 객체의 겟터 메서드를 호출하는 코드 생성.

4. <jsp:include>
정적(HTML, text) 또는 동적 자원(서블릿/JSP)을 인클루딩 하는 자바 코드를 생성.

5. <jsp:forward>
현재 페이지의 실행을 멈추고 다른 정적 자원이나 동적 자원으로 포워딩하는 자바 코드 생성

6. <jsp:param>
jsp:include, jsp:forward, jsp:params의 자식 태그로 사용할 수 있다. ServletRequest 객체에 매개 변수를 추가하는 코드를 생성한다.

7. <jsp:plugin>
Object 또는 Embed HTML 태그를 생성.

8. <jsp:element>
임의의 XML 태그나 HTML 태그를 생성.
```

### JSP 액션 태그 <jsp:useBean>

<jsp:useBean> 액션 태그는 application, session, request, page 보관소에 저장된 자바 객체를 꺼낼 수 있다.

```
<jsp:useBean id="이름" scope="page|request|session|application" class="클래스명" type="타입명" />
```

사용예

MemberList.jsp

```html
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
</html>
```

위 코드를 jsp 액션 태그로 바꾸면 아래와 같다.

```html
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
    <jsp:include page="/Header.jsp" />
    <h1>회원 목록</h1>
    <p><a href="add">신규회원</a></p>
    
    <jsp:useBean id="members" 
                scope="request" 
                class="java.util.ArrayList" 
                type="java.util.ArrayList<spms.vo.Member>" />
    <%
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
</html>
```

Header.jsp 

```html
<%@page import="spms.vo.Member"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<jsp:useBean id="member"
            scope="session"
            class="spms.vo.Member" />
<div style="background-color:#00008b; color:#ffffff; height: 20px; padding: 5px;">
SPMS(Simple Project Management System)
    <% if (member.getEmail() != null) { %>
    <span style="float:right;">
    <%= member.getName() %>
    <a style="color:white;" href="<%=request.getContextPath()%>/auth/logout">로그아웃</a>
    </span>
    <% } %>
</div>
```


## EL 사용하기

EL(Expression Language)은 콤마와 대괄호를 사용하여 자바 빈의 프로퍼티나 맵, 리스트, 배열의 값을 보다 쉽게 꺼내게 해주는 기술입니다. 또한 스태틱으로 선언된 메서드를 호출할 수도 있다. JSP에서는 주로 보관소에 들어 있는 값을 꺼낼 때 사용한다.

EL은 ${}와 #{}를 사용하여 값을 표현.

1. ${} : 즉시적용. JSP가 실행될 때 JSP페이지에 즉시 반영된다.
2. #{} : 지연적용. 시스템에서 필요하다고 판단될 때 그 값을 사용. 객체 프로퍼티의 값을 꺼내기보다는 사용자가 입력한 값을 객체의 프로퍼티에 담는 용도로 많이 사용된다.

```
${member.no} 또는 ${member["no"]}

자바 코드로 표현하면 다음과 같다.

Member obj = (Member)pageContent.findAttribute("member");
int value = obj.getNo();
```

findAttribute()는 JspContext - ServletRequest - HttpSession - ServletRequest - null의 순서로 보관소를 뒤져서 객체를 찾는다. 없다면 null을 반환한다.

### EL에서 검색 범위 지정

jsp:useBean 처럼 네 군데 보관소에서 값을 꺼낼 수 있다. jsp:useBean과 다른 점은 EL로는 객체를 생성할 수 없다는 것이다.

```
pageScope : JspContext
requestScope : ServletRequest
sessionScope : HttpSession
applicationScope : ServletRequest
```

ServletRequest 보관소에서 값을 꺼내는 EL 코드

```
${requestScope.member.no}

자바 코드로 표현하면 다음과 같다.

Member obj = (Member)request.getAttribute("member");
int value = obj.getNo();
```

### EL 실습

배열에서 값 꺼내기

```html
<body>
    <%
        pageContext.setAttribute("scores", new int [] {90, 80, 70, 100});
    %>
    ${scores[2] }
</body>
```

List 객체에서 값 꺼내기

```html
<body>
    <%
        List<String> nameList = new LinkedList<String>();
        nameList.add("홍길동");
        nameList.add("임꺽정");
        pageContext.setAttribute("nameList", nameList);
    %>
    ${nameList[1] }
</body>
```

자바 객체에서 프로퍼티 값 꺼내기

```html
<body>
    <%
        pageContext.setAttribute("member", new Member()
                                .setNo(100)
                                .setName("홍길동")
                                .setEmail("hong@test.com") );
    %>
    ${member.email}
</body>
```

ResourceBundle 객체에서 값 꺼내기

```java
import java.util.ListResourceBundle;

public class MyResourceBundle_ko_KR extends ListResourceBundle{

    @Override
    protected Object[][] getContents() {
        return new Object[][] {
            {"OK", "확인"},
            {"Cancel", "취소"},
            {"Reset", "재설정"},
            {"Submit", "제출"}
        };  
    }
}
```

```html
<body>
    <%
        pageContext.setAttribute("myRB", ResourceBundle.getBundle("spms.vo.MyResourceBundle"));
    %>
    ${myRB.OK}
</body>
```
 



































