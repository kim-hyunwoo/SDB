# JSP 액션 태그, EL, JSTL

## Jsp 액션 태그

JSP에서 기본으로 제공하는 태그들의 집합을 JSP 액션이라 한다.

```
1. <jsp:useBean>
자바 인스턴스를 준비. 보관소에서 자바 인스턴스를 꺼내거나, 자바 인스턴스를 새로 만들어 보관소에 저장하는 코드를 생성한다. 자바 인스턴스를 자바 빈(Java Bean)이라고 부른다.

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

> <jsp:useBean> 액션 태그는 application, session, request, page 보관소에 저장된 자바 객체를 꺼낼 수 있다.
