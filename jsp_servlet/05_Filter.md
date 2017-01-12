# 필터 사용하기

필터란? 서블릿 실행 전 후에 어떤 작업을 하고자 할 때 사용하는 기술이다. 예를 들어 클라이언트가 보낸 데이터의 암호를 해제한다거나, 서블릿이 실행되기 전에 필요한 자원을 미리 준비한다거나, 서블릿이 실행될 때마다 로그를 남긴다거나 하는 작업을 필터를 통해 처리할 수 있다.

필터의 실행 과정

> 클라이언트 -> 서블릿 컨테이너 -> 필터 1 -> 필터 2 -> 서블릿

이런 작업들을 서블릿에 담는다면 서블릿마다 해당 코드를 삽입해야 하고, 필요가 없어지면 그 코드를 삽입한 서블릿을 모두 찾아서 제거해야하므로 관리가 어렵다.

### 실습

#### 필터에서 POST 요청으로 넘어온 매개변수의 문자집합을 설정

POST 요청의 경우 서버로 보내는 데이터는 메시지 바디에 있는데 서블릿에서 이 데이터를 꺼내려면 getParameter()를 호출해야 한다. 만약 메시디 바디에 한글과 같은 멀티바이트 문자가 있을 때 그냥 getParameter()를 호출하면 깨진다. 이를 해결하기 위해 setCharacterEncoding()을 호출하여 메시지 바디의 데이터가 어떤 문자 집합으로 인코딩 되었는지 설정해야 한다.

```java
request.setCharacterEncoding("utf-8");
```

그러나 각 서블릿 마다 위 코드를 작성하는 것은 매우 번거롭다. 이럴 때 서블릿 필터를 사용하면 간단히 처리할 수 있다.

##### 필터 만들기

javax.servlet.Filter 인터페이스 구현

> init(), doFilter(), destroy()

1. spms.filters라는 패키지 생성
2. CharacterEncodingFilter 클래스 생성 및 Filter 인터페이스 구현
3. 필터가 되려면 반드시 javax.servlet.Filter 인터페이스를 구현해야 한다.

필터는 서블릿과 마찬가지로 웹 애플리케이션이 실행되는 동안 계속 유지된다.

```java
public class CharacterEncodingFilter implements Filter{
    
    FilterConfig filterConfig;
    
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.filterConfig = filterConfig;
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        request.setCharacterEncoding(filterConfig.getInitParameter("encoding"));
        // 서블릿이 실행되기 전에 메시지 마디의 문자집합을 설정        
        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {

    }
}
```

##### init

필터 객체가 생성된 후 준비 작업을 위해 딱 한 번 호출된다. Servlet 인터페이스의 init()과 같은 용도이다. 이 메서드의 매개변수는 FilterConfig 객체로 이 객체를 통해 필터 초기화 매개변수의 값을 꺼낼 수 있다.

예제 코드에선 이 객체를 doFilter()에서 사용하기 위해 인스턴스 변수에 저장했다.
```java
    FilterConfig filterConfig;
    
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.filterConfig = filterConfig;
    }
```

##### doFilter

필터와 연결된 URL에 대해 요청이 들어오면 doFilter()가 항상 호출된다. 이 메서드에 필터가 할 일을 작성하면 된다.

- chain 매개변수는 다음 필터를 가리킨다.
- chain.doFilter()는 다음 필터를 호출한다. 다음 필터가 없다면 내부적으로 서블릿의 service()를 호출한다.
- 만약 서블릿이 실행되기 전에 처리할 작업이 있다면 chain.doFilter() 전에 작성해야 한다.
- 서블릿이 실행된 후 처리할 작업은 chain.doFilter() 이후에 작성하면 된다.

```java
@Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        //배치파일이나 어노테이션에서 encoding의 값을 가져옴
        request.setCharacterEncoding(filterConfig.getInitParameter("encoding"));
        chain.doFilter(request, response);
}
```

##### destory

웹 애플리케이션을 종료하기 전에 필터들에 대해 destory()를 호출하여 마무리 작업을 할 수 있는 기회를 준다.

### 필터의 구동 순서

1. 서블릿 컨테이너는 웹 애플리케이션을 시작할 때 DD 파일에 등록된 필터의 인스턴스를 생성
2. 생성된 필터에 준비 작업을 할 수 있도록 init() 호출
3. 클라이언트의 요청이 들어오면 그 요청에 해당하는 필터의 doFilter() 메서드를 호출
4. doFilter()에서는 필터로서 해야 할 작업을 실행하고 다음 필터의 doFilter() 메서드를 호출. 마지막 필터까지 이 과정 반복
5. 마지막 필터는 내부적으로 서블릿의 service() 호출
6. service() 호출이 끝나면 service()를 호출했던 이전 필터로 돌아간다. 이런식으로 반복해서 제일 처음 호출된 필터까지 돌아간다.
7. 마지막으로 클라이언트에게 응답 결과를 보낸다.

적용 사례

- service() 호출 사전 작업 : doFilter() 전에 처리하는 작업

클라이언트가 보낸 데이터의 문자 집합을 설정한다던가, 압축 데이터 풀기, 암호화된 데이터를 원래 데이터로 복원하기, 로그 남기기
사용자 검증하기, 사용권한 확인하기 등의 작업을 수행.

- service() 호출 사후 작업 : doFilter() 후에 처리하는 작업

응답 데이터 압축하기, 응답 데이터 암호화하기, 응답 데이터를 다른 형식으로 변환하기 등

### 필터의 배치

서블릿과 마찬가지로 DD파일과 어노테이션 방식의 배치 방법이 있다.

##### DD 파일

```xml
  <!-- 필터 선언 -->
  <filter>
  <!-- 필터의 별명 설정. 필터와 URL을 연결할 때 사용 -->
    <filter-name>CharacterEncodingFilter</filter-name>
    <!-- 필터의 클래스 이름은 QName이어야 한다. -->
    <filter-class>spms.filters.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
  </filter>
  
  <!-- 필터 URL 맵핑 : 어떤 요청에 대해 필터를 적용할 것인지 설정 -->
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <!-- 필터가 적용되어야 하는 URL 설정. /*는 모든 요청을 의미한다. -->
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```

컨텍스트 매개변수 다음에 필터를 선언하는 태그를 둔다(Servlet2.4 이전 버전 호환을 위해서)

##### 어노테이션 방식

```java
@WebFilter(
            urlPatterns="/*",
            initParams= {
                    @WebInitParam(name="encoding", value="utf-8")
            })
public class CharacterEncodingFilter implements Filter{
    
    FilterConfig filterConfig;
    
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.filterConfig = filterConfig;
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
            throws IOException, ServletException {
        request.setCharacterEncoding(filterConfig.getInitParameter("encoding"));
        chain.doFilter(request, response);
    }

    @Override
    public void destroy() {

    }
}
```

## 적용

Servlet에서 setCharacterEncoding() 호출부분을 제거하고 테스트 해보면 정상적으로 출력됨을 알 수 있다.

































     
















