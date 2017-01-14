# 06_MVC_JSP.md

서블릿의 단점을 보완하기 위해 JSP 등장. 클라이언트의 요청 처리를 서블릿 홀로 담당하는 올인원 방식은 유지보수가 어려워 운영비용이 증가하게 된다. 때문에 MVC 아키텍처가 등장하게 된다.

### MVC 각 컴포넌트의 역할

#### 컨트롤러

클라이언트의 요청을 받았을 때 그 요청에 대해 실제 업무를 수행하는 모델 컴포넌트를 호출한다. 클라이언트가 보낸 데이터가 있다면 모델을 호출할 때 전달하기 쉽게 데이터를 적절히 가공한다. 모델이 업무 수행을 완료하면, 그 결과를 가지고 화면을 생성하도록 뷰에게 전달한다. 즉 클라이언트 요청에 대해 모델과 뷰를 결정하여 전달하는 일을 한다. 일종의 조정자

#### 모델

데이터 저장소와 연동하여 사용자가 입력한 데이터나 사용자에게 출력할 데이터를 다루는 일을 한다. 특히 여러 개의 데이터 변경 작업을 하나의 작업으로 묶은 트랜잭션을 다루는 일도 한다.

#### 뷰

모델이 처리한 데이터나 그 작업 결과를 가지고 사용자에게 출력할 화면을 만드는 일을 한다. 즉, 뷰 컴포넌트는 HTML, CSS, JS를 이용해서 웹 브라우저가 출력할 UI를 만든다.


### MVC의 이점

#### 높은 재사용성, 넓은 융통성

- 룩앤필을 쉽게 교체할 수 있다. 화면 생성 부분을 별도의 컴포넌트로 분리했기 때문에, 컨트롤러나 모델에 관계없이 뷰 교체만으로 배경색이나 모양, 레이아웃, 글꼴 등 사용자 화면을 손쉽게 바꿀 수 있다.

- 원 소스 멀티 유즈를 구현할 수 있다. 모델 컴포넌트가 작업한 결과를 다양한 뷰 컴포넌트를 통하여 PDF나 HTML, XML, JSON 등 클라이언트가 원하는 형식으로 출력할 수 있다.

- 코드를 재사용할 수 있다. 화면을 바꾸거나 데이터 형식을 바꾸더라도 모델 컴포넌트는 그대로 재사용할 수 있다.


#### 빠른 개발, 저렴한 비용
- 다른 프로젝트에서도 모델 컴포넌트를 재사용할 수 있기 때문에 개발 속도가 빨라진다.
자바 개발자는 컨트롤러와 모델 개발에 전념하고, JSP HTML 개발자는 뷰 개발에 전념할 수 있기 때문에 개발속도가 빠르다

- 소스 코드를 역할에 따라 여러 컴포넌트로 쪼개게 되면, 그 컴포넌트의 난이도에 따라 좀 더 낮은 수준의 개발자를 투입할 수 있어서 전체적인 개발 및 유지보수 비용을 줄일 수 있다.

### MVC의 구동원리

1. 웹 브라우저가 웹 어플리케이션 실행을 요청. 웹 서버가 그 요청을 받아 WAS(서블릿 컨테이너; 톰캣)에 넘겨줌 서블릿 컨테이너는 URL을 확인해서 그 요청을 처리할 서블릿을 찾아서 실행

2. 서블릿은 실제 업무를 처리하는 모델 자바 객체의 메서드 호출. 만약 웹 브라우저가 보낸 데이터를 저장하거나 변경해야 한다면 그 데이터를 가공하여 값 객체(Value Object)를 생성하고, 모델 객체의 메서드를 호출할 때 인자값으로 넘긴다. 모델 객체는 엔터프라이즈 자바빈 또는 일반 자바 객체(POJO) 일수도 있다.

3. 모델 객체는 JDBC를 사용하여 매개변수로 넘어온 값 객체를 데이터베이스에 저장하거나, 데이터베이스로부터 질의 결과를 가져와서 값 객체로 만들어 반환한다. 이렇게 값 객체는 객체와 객체 사이에 데이터를 전달하는 용도로 사용하기 때문에 ‘데이터 전송 객체(DTO)’라고 부른다.

4. 서블릿은 모델 객체로부터 반환받은 값을 JSP에 전달한다.

5. JSP는 서블릿으로부터 전달받은 값 객체를 참조하여 웹 브라우저가 출력할 결과 화면을 만든다. 그리고 웹 브라우저에 출력하여 요청 처리를 완료한다.

6. 웹 브라우저는 서버로부터 받은 응답 내용을 화면에 출력한다.

## 뷰 컴포넌트와 JSP

JSP 기술의 가장 중요한 목적은 컨텐츠를 출력하는 코딩을 단순화 하는 것. JSP가 직접 실행되는 것이 아니라, JSP로부터 만들어진 서블릿이 실행된다.

### HttpJspPage 인터페이스

JSP엔진은 JSP 파일로부터 서블릿 클래스를 생성할 때 HttpJspPage 인터페이스를 구현한 클래스를 만든다. JspPage인터페이스는 Servlet인터페이스를 구현하기 때문에 HttpJspPage를 구현한다는 것은 Servlet인터페이스도 구현한다는 것이다.
결국 해당 클래스는 서블릿이 될 수밖에 없다.

### JSP의 주요 구성 요소

크게 두 가지로 나눌 수 있음. 템플릿 데이터와 JSP 전용 태그. 템플릿 데이터는 클라이언트로 출력되는 컨텐츠(HTML, JS, CSS, JSON, XML, plain text 등) JSP 전용 태그는 특정 자바 명령문으로 바뀌는 태그.

## 계산기 실습

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
    String v1 = "";
    String v2 = "";
    String result = "";
    String [] selected = {"", "", "", ""};
    
    //값이 있을 때만 꺼낸다.
    if(request.getParameter("v1") != null){
        v1 = request.getParameter("v1");
        v2 = request.getParameter("v2");
        String op = request.getParameter("op");
        
        result = calculate(
                    Integer.parseInt(v1),
                    Integer.parseInt(v2),
                    op );
        
        if("+".equals(op)) {
            selected[0] = "selected";
        } else if("-".equals(op)) {
            selected[1] = "selected";
        } else if("*".equals(op)) {
            selected[2] = "selected";
        } else if("/".equals(op)) {
            selected[3] = "selected";
        }
    }
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <h2>JSP 계산기</h2>
    <form action="calc.jsp" method="get">
        <input type="text" name = "v1" size = "4" value = "<%=v1%>">
        <select name="op">
            <option value="+" <%=selected[0]%>>+</option>
            <option value="-" <%=selected[1]%>>-</option>
            <option value="*" <%=selected[2]%>>*</option>
            <option value="/" <%=selected[3]%>>/</option>
        </select>
        <input type="text" name="v2" size="4" value="<%=v2%>" >
        <input type="submit" value="=">
        <input type="text" size="8" value="<%=result%>"><br>
    
    </form>
</body>
</html>
<%!
private String calculate(int a, int b, String op) {
    int r = 0;
    
    if("+".equals(op)) {
        r = a + b;
    } else if("-".equals(op)) {
        r = a - b;
    } else if("*".equals(op)) {
        r = a * b;
    } else if("/".equals(op)) {
        r = a/b;
    }
    return Integer.toString(r);
}
%>
```

## JSP 전용 태그

### 지시자

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
```

> <%@ 지시자 속성="값" 속성="값" ... %>

지시자나 속성에 따라 특별한 자바 코드를 생성한다. page, taglib, include가 있다.

#### page 지시자

JSP 페이지와 관련된 속성을 정의할 때 사용하는 태그이다.

1. language 속성 : 스크립트릿이나 표현식, 선언부를 작성할 때 사용할 프로그래밍 언어를 지정한다.
2. contentType 속성 : 출력할 데이터의 MIME 타입과 문자 집합을 지정.
3. pageEncoding 속성 : 출력할 데이터의 문자 집합을 지정.

### 스크립트릿

```jsp
<% ... %>
```

JSP 페이지 안에 자바 코드를 넣을 때는 스크립트릿 태그 안에 작성한다. 서블릿 파일 생성시 그대로 복사된다.

### 선언문

```jsp
<%! ... %>
```

서블릿 클래스의 멤버(변수, 메서드)를 선언할 때 사용하는 태그이다. 선언문의 위치는 어디든 상관없다. 왜냐하면 선언문은 \_jspService() 메서드 안에 복사되는 것이 아니라, \_jspService() 밖의 클래스 블록 안에 복사되기 때문이다.

### 표현식

```jsp
<%= ... %>
```

표현식 태그는 문자열을 출력할 때 사용한다. 따라서 표현식 안에는 결과를 반환하는 자바 코드가 와야 한다.

## JSP 내장 객체

별도의 선언 없이 JSP 페이지에서 사용할 수 있는 자바 객체. 예) request 객체

> request, response, pageContext, session, application, config, out, page, exception

스크립트릿 <% %>과 표현식 <%= %>에서 작성한 자바 코드는 _jspService() 메서드로 복사될 때 JSP 내장 객체를 선언한 문장 뒤에 놓인다. 이런 이유로 별도 선언 없이 스크립트릿과 표현식에서 JSP 내장 객체를 사용할 수 있다.

























