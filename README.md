# web_servlet_JSP

## 1. 웹 서버 프로그램이란?

웹 서버(Web Server)는 HTTP를 통해 웹 브라우저에서 요청하는 HTML 문서나 오브젝트(이미지 파일 등)을 전송해주는 서비스 프로그램을 말한다.

일반적으로 사용자의 입장을 클라이언트 (Client) 라고 부르고, 제공하는 입장을 서버 (Server)라고 부른다. 클라이언트가 데이터를 받고 통신하는 어플리케이션은 웹 브라우저 (Web Browser) 이며 웹 브라우저를 이용해서 웹 서버로 요청을 보낸다.

웹 브라우저가 웹서버에게 자료를 요청하고 요청에 대한 응답을 처리하기 위해 HTTP (Hyper Text Transfer Protocol)을 사용한다.

### 1.1 WAS(Web Application Server)란?

웹 브라우저와 같은 클라이언트로 부터 웹 서버가 요청을 받으면 애플리케이션에 대한 로직을 실행해 웹 서버로 다시 반환해주는 소프트웨어

웹 서버와 DBMS 사이에서 동작하는 미들웨어로써, 컨테이너 기반으로 동작

웹 서버와 WAS의 차이점은 웹 서버의 경우 정적인 컨텐츠 (HTML, CSS)를 요청받아 처리하고 WAS는 동적인 컨텐츠(JSP, ASP 등)를 요청받아 처리

## 2. 톰캣 설치하기

  아파치는 웹 서버이고 톰캣은 WAS 이다.

- 아파치(apache)란?

세계에서 가장 많이 쓰는 웹 서버중 하나이며, 아파치 소프트웨어 재단에서 관리하는 HTTP 웹 서버이다.

- 톰캣(Tomcat)이란?

톰캣은 아파치 소프트웨어 재단의 웹 어플리케이션 서버(와스)로서, 자바 서블릿을 실행키고 JSP코드가 포함되어 있는 웹 페이지를 만들어준다. 

### 2.1 아파치 톰캣 다운로드 

http://tomcat.apache.org/

### 2.2 JAVA_HOME 환경변수 확인

### 2.3 실행

apache-tomcat-9.0.54\bin -> startup 실행

### 2.4 브라우저 접속

http://localhost:8080/

### 2.5 웹 문서 추가하기

문서를 보관하는 home 디렉토리는 apache-tomcat-9.0.54\webapps\ROOT 이고 여기에 문서를 넣고

브라우저로 http://localhost:8080/nana.txt 이렇게 요청을 하면 home 디렉토리에 있는 문서를 보여줌

같은 계통의 네트워크를 사용 중이면 http:// 자신의 아이피:8080/nana.txt. 로 접속가능

### 2.6 Context 사이트 추가

context는 Home 디렉토리가 아닌 다른 디렉토리에 있는 문서에 가상 경로를 부여해서 Home 디렉토리에 있는 것처럼 하는 것

- 설정 방법 <br>
conf 디렉토리 server_xml 파일의 Host 태그 안에 \<Context path="it" docBase = "문서 경로" privileged="true"> 을 추가 (it라는 가상경로 부여)

그러면 문서 이름이 news.txt 라고 할 때 주소에 http://localhost:8080/it/news.txt를 입력하면 news.txt 를 불러올 수 있다.


## 3. Servlet 프로그램 만들기

### 서블릿이란

- 자바를 사용하여 웹을 만들기 위해 필요한 기술
- 예를 들어, 유저가 로그인 시도시에 아이디와 비밀번호를 입력하고 로그인 버튼을 누른다.
- 이 때, 서버가 클라이언트에서 들어오는 아이디와 비밀번호를 확인하고 다음 페이지를 띄워주는데 이러한 역할을 수행하는 것이 Servlet이다.

![image](https://user-images.githubusercontent.com/81665608/139850953-f14d5678-d5cc-4e0b-bd2c-920f4537e91a.png)

### 3.1 서블릿 객체 생성과 실행 방법

- 컴파일된 파일을 apache-tomcat-9.0.54\webapps\ROOT\WEB-INF 경로에 classes 폴더를 만들어서 넣어야한다.
- 패키지가 있다면 패키지 폴더도 전부 들어가야 한다.
- WEB-INF 폴더는 사용자에 의해서 직접적으로 요청될 수 없는 디렉토리
- 서버쪽에서만 사용 가능

### 실행방식
![image](https://user-images.githubusercontent.com/81665608/139854934-bf9ce1ba-3671-4890-98c8-4d2cb8ba6f45.png)

위에 예시의 서블릿 맵핑은 WEB-INF 폴더의 web.xml 파일에web-app 태그 안에 해준다. 만약 패키지라면 Nana 앞에 패키지도 전부 써준다.

요청을 받은 웹 서버는 파일을 찾아보고 없으면 WAS에게 넘기고 WAS는 자기 맵핑 정보를 뒤져서 거기에 맞는 서블릿 코드를 실행한다.

### 3.2 서블릿 문자열 출력

service 라는 함수는 두 개의 인자를 전해준다. request, response 객체

그 중 response 객체를 이용해서 출력을 한다.

출력이나 입력은 Stream을 쓰는게 가장 기본적

    public class Nana extends HttpServlet
    {
      public void service(HttpServletRequest request
          , HttpServletResponse response)
          throws IOException, ServletException
      {
        OutputStream os = response.getOutputStream();
        PrintStream out = new PrintStream(os, true);
        out.println("Hell Servlet");
      }
    }
 
문자열인데 다국어면 Writer 계열을 쓴다.

    PrintWriter out = response.getWriter();
    out.println("Hello Servlet!!");


## 4. 이클립스를 이용한 서블릿 프로그래밍

- web 프젝트를 만들 때 자신의 WAS 프로그램을 선택하고 만든다.
- 프로젝트를면 여러 디렉토리 중 WebContent가 home 디렉토리 이다.
- 자바 코드는 JavaResources의 src에 만든다.
- 프로젝트 오른쪽 클릭 -> 속성 -> Web Project Settings 에서 Context root로 가상 경로를 지정하면 자신의 프로젝트 이름으로 요청을 하지 않도록 할 수 있다.
- 서블릿 코드 작성 후 home 디렉토리 WEB-INF 안에 web.xml 파일을 만들어 맵핑을 한 하고 실행하면 3. 과정에서 했던 처리를 알아서 해준다.


###4.1 Annotation을 이용한 URL 맵핑

![image](https://user-images.githubusercontent.com/81665608/140028146-7aa560f8-7ca1-47f9-a4ce-54e358993e54.png)

![image](https://user-images.githubusercontent.com/81665608/140028297-7598b006-ac4d-4ad3-83e4-05c42d9cd254.png)

- false : web.xml 외에도 설정한게 있으니 찾아보라는 의미
- true : 모든 설정은 web.xml에 했다는 의미

## 5. 서블릿 출력 형식을 지정해야 하는 이유

![image](https://user-images.githubusercontent.com/81665608/140032523-c28674bc-db04-45c4-b2d5-1d25c09a4427.png)

### 5.1 한글 출력하기

1. 한글은 2바이트 씩 묶어서 표현이 되는데 톰캣의 경우 1바이트씩 보내고 브라우저는 이게 무슨 말인지 몰라  ?로 보여준다.
2. 정상적으로 보냈지만 브라우저가 잘못 해석한 경우
![image](https://user-images.githubusercontent.com/81665608/140033549-37e303bf-4828-4ba0-a4e6-6d5375641885.png)


### 해결방법

1. 응답 도구를 이용해서 인코딩 방식을 지정해준다.

  response.setCharacterEncoding("UTF-8"); // UTF-8로 인코딩 할 것이다.
  
2. 브라우저에게 UTF-8 로 읽어야 한다고 알려줘야한다.

  response.setContentType("text/html; charset=UTF-8");  // html 형식과 UTF-8 로 읽어라



  @WebServlet("/hi")
  public class Nana extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

      response.setCharacterEncoding("UTF-8");
      response.setContentType("text/html; charset=UTF-8");

      PrintWriter out = response.getWriter();
      for(int i = 0 ; i < 100 ; i++) {
        out.println("Hello ~~~ 안녕<br>");			
      }
    }	

  }
  

## 6. GET 요청과 쿼리스트링

- 쿼리 스트링 : 서버에게 문서를 요청하면서 추가적인 옵션을 추가하는것

![image](https://user-images.githubusercontent.com/81665608/140274874-d0e0f5f3-80b7-4646-b6a0-9f26d8debf7d.png)


### 6.1 쿼리스트링 기본 값

![image](https://user-images.githubusercontent.com/81665608/140282345-1bc8d223-80b3-42b4-aeaa-9effda5c4f18.png)

하지만 빈 문자열이 오는경우 Integer.parselnt(temp)에서 에러가 발생하므로

  if(temp_ != null && !temp.equals("")) // 빈문자열이 아니다 라는 조건도 추가
    cnt = Integer.parseInt(temp);

  for(int i=0; i<cnt; i++)
    out.println(i + 1)+": 안녕 Servlet!!<br>");

#### index.html

  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8">
  <title>Insert title here</title>
  </head>
  <body>
    환영합니다.<br>
    <a href = "hi">인사하기</a> <br>
    <a href = "hi?cnt=3">인사하기</a><br>
  </body>
  </html>

### 6.2 사용자 입력을 통한 GET 요청

![image](https://user-images.githubusercontent.com/81665608/140290108-a170585e-6f44-4f37-985f-c393ce16daa1.png)

![image](https://user-images.githubusercontent.com/81665608/140290814-a5774b36-4acf-4f0b-a8c8-3291a977a7be.png)

### 6.3 입력할 내용이 많은 경우는 POST 요청
   
    // form 속성으로 아래와 같이 method = "post" 를 추가하면
    // url를 통해서 요청하지 않고 요청 바디에 붙여서 요청하게 된다.
    // 그러면 주소창에 쿼리 스트링의 값이 노출 되지 않는다.
    <form action = "notice-reg" method = "post">
    
POST를 이용해서 요청하는 경우 한글이 깨지는 현상이 있는데 

    request.setCharacterEncoding("UTF-8"); // 요청도구를 이용해서 UTF-8로 셋팅해준다.
  
  
## 7. 서블릿 필터

- 서블릿 필터란?

사용자 인증이나 로깅과 같은 기능들은 모든 서블릿이나 JSP가 공통적으로 필요로 함.

이러한 공통적인 기능들을 서블릿이 호출되기 전에 수행(전처리)되게 하고 싶거나

서블릿이 호출 되고 난 뒤에 수행(후처리) 하고 싶으면 공통적인 기능들을 서블릿 필터로 구현하면 된다.

![image](https://user-images.githubusercontent.com/81665608/140308655-8f3635f0-f8eb-4118-8e08-457d7f2bc86d.png)

### 7.1 필터 클래스 만들기

    @WebFilter("/")   // 필터도 이런식으로 맵핑 가능
    public class CharacterEncodingFilter implements Filter { // 인터페이스 필터 추가

      @Override
      public void doFilter(ServletRequest request,
          ServletResponse response,
          FilterChain chain)
          throws IOException, ServletException {

        System.out.println("before filter");
        request.setCharacterEncoding("UTF-8");
        
        chain.doFilter(request, response); // 흐름을 넘겨서 다음 필터 또는 서블릿이 실행하게함
        
        // 넘겼던 실행 결과가 돌아오면 아래 코드가 실행됨
        System.out.println("after filter");
        
        // response.setCharacterEncoding("UTF-8");
		    // response.setContentType("text/html; charset=UTF-8");
        // 위의 두 문장의 경우
        // response는 응답이라서  doFilter 이후에 넣어야 하는데
        // 위의 것을 추가하면 모든 응답에 콘텐츠 타입이
        // css 나 image 이여도 text/html로 고정이 된다.
        // 그러므로 사용목적에 맞게 잘 사용

      }

    }

### web.xml 에 필터 추가하는 방법

    <filter>  
      <filter-name>characterEncodingFilter</filter-name>
      <filter-class>com.newlecture.web.filter.CharacterEncodingFilter</filter-class>
    </filter>
    <filter-mapping>
      <filter-name>characterEncodingFilter</filter-name>
      <url-pattern>/*</url-pattern> // 모든 URL에 대해 적용하겠다.
    </filter-mapping>


## 9. 여러 개의 Submit 버튼 사용하기

	<form action="add" method="post"> 
		<div>
			<label>숫자1</label>
			<input name = "num1" type="text">
			<label>숫자2</label>
			<input name = "num2" type="text">
			<input type = "submit" name = "operator" value="덧셈"> // /add?operator="덧셈" 이랑 같음
			<input type = "submit" name = "operator" value = "뺄셈">
		</div>	
	</form>
	
## 10. 입력 데이터 배열로 받기

사용자 입력을 받을 때 입력이 무수히 많으면 일일이 name을 정해줄 수 없다.<br>
그럴 경우 name을 통일하면 배열로 값을 보내는데 서블릿 쪽에서 배열로 받으면 된다.

	<input name = "num" type="text">
	<input name = "num" type="text">
	<input name = "num" type="text">
	
	// 서블릿
	String[] num_ = req.getParameterValues("num");
	
	
	
	
	
	
