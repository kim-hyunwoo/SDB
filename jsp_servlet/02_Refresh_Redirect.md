# 리프래시 & 리다이렉트

## 리프래시

일정 시간이 지난 뒤 자동으로 서버에 요청을 보내는 방법. 작업 결과를 출력한 후 다른 페이지로 이동할 때는 리프래시를 사용

### 응답 헤더를 이용한 리프래시

doPost() 메서드에서 리프래시 정보를 응답 헤더에 추가한다.

```java
response.addHeader("Refresh", "1;url=list");
```

### HTML의 meta 태그를 이용한 리프래시

리프래시 정보는 HTML 본문에 포함시켜 보낼 수 있다. HTML의 <head> 태그 안에 리프래시를 설정하는 <meta> 태그를 추가한다.

```java
out.println("<html><head><title>회원등록결과</title>");
out.println("<meta http-equiv='Refresh' content='1;url=list’/></head>");
```

## 리다이렉트

작업 결과를 출력하지 않고 다른 페이지로 이동한다면, 리다이렉트로 처리하면 된다.

```java
response.sendRedirect("list");
```

