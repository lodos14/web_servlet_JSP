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



























