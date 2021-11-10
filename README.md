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
	
## 11. 상태 유지를 위한 방법

1. application : 서블릿 컨텍스트(Context) 저장소 application 전역에서 사용할 수 

		ServletContext application = req.getServletContext();
		application.setAttribute("value", v);
		int x = (Integer)application.getAttribute("value"); // 반환 타입은 오브젝트
		
		//EX) 예제
		@WebServlet("/calc")
		public class AddApp3 extends HttpServlet  {

			@Override
			protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

				ServletContext application = req.getServletContext();
				resp.setCharacterEncoding("UTF-8");
				resp.setContentType("text/html; UTF-8");

				PrintWriter out = resp.getWriter();


				String v_ = req.getParameter("v"); // 배열로 받는 경우
				String op = req.getParameter("operator");

				int v = 0;	

				if (!v_.equals("")) {
					v = Integer.parseInt(v_);
				}

				if(op.equals("=")) {

					int x = (Integer)application.getAttribute("value"); // object 로 반환하기 때문에 형변환
					int y = v;
					String operator = (String)application.getAttribute("op");
					int result = 0;

					if(operator.equals("+")) {
						result = x + y;
					} else {
						result = x - y;
					}
					out.println(result);

				}else {
					application.setAttribute("value", v);
					application.setAttribute("op", op);			
				}

			}

		}
		
2. session - 사용자마다 개별공간이 있음 

		HttpSession session = req.getSession();
		session.setAttribute("value", v);
		int x = (Integer)session.getAttribute("value"); 
		
3. cookie - 상태 값을 서버에 저장하는게 아니라 가지고 다님
		
		// 쿠키 보내기
		Cookie valuecookie = new Cookie("value", Integer.toString(v)); // 쿠키로 보낼수 있는 건 문자열
		resp.addCookie(valuecookie);
		
		// 쿠키 가져오기 - 쿠키의 데이터 형태는 배열
		Cookie[] cookies = req.getCookies();
		for(Cookie c : cookies) {			
			if(c.getName().equals("value")) {
				x =  Integer.parseInt(c.getValue());
				break;
			}
		}

### Cookie의 path 옵션

	Cookie valuecookie = new Cookie("value", Integer.toString(v)); 
	Cookie opCookie = new Cookie("op", op);
	
	// 서블릿 문서가 많기 때문에 쿠키를 저장할 때마다 모든 쿠키를 불러오는 걸 방지
	// 원하는 경로의 서블릿에게만 쿠키를 가져와라
	valuecookie.setPath("/");
	opCookie.setPath("/");
	
	resp.addCookie(valuecookie);
	resp.addCookie(opCookie);
	
### Cookie의 MaxAge 옵션

- 쿠키의 만료날짜 설정

		valuecookie.setMaxAge(1000); // 쿠키의 만료 -> 1000초  24시간은 24*60*60


### 11.1 WAS가 현재 사용자(Session)을 구분하는 방식

![image](https://user-images.githubusercontent.com/81665608/140478188-b39002eb-5352-489d-9c67-d47973a84b5d.png)

![image](https://user-images.githubusercontent.com/81665608/140478953-f8264392-9894-40c3-a3d9-e6bc49988a8b.png)

	
### 11.2 각 저장소 정리	
1. application 
	- 사용범위 : 전역 범위에서 사용하는 저장 공간
	- 생명주기 : WAS가 시작해서 종료할 때 까지
	- 저장위치 : WAS 서버의 메모리

2. session
	- 사용범위 : 세션 범위에서 사용하는 저장 공간
	- 생명주기 : WAS가 시작해서 종료할 때 까지
	- 저장위치 : WAS 서버의 메모리

3. cookie - 특정 URL이나 오랜기간 가져가야 하는 경우 사용
	- 사용범위 : Web Browser별 지정한 path 범주 공간
	- 생명주기 : Browser에 전달한 시간부터 만료시간까지
	- 저장위치 : Web Browser의 메모리 또는 파일

## 12. 서버에서 페이지 전환해주기

	resp.sendRedirect("add3.html"); // 서버에서 지정한 페이지로 전환해줌
	
### 12.1 동적인 페이지 서블릿으로 만들기

	@WebServlet("/calcpage")
	public class CalcPage extends HttpServlet  {

		@Override
		protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

			Cookie[] cookies = req.getCookies(); 
			String exp = "0";

			if(cookies != null) { // 쿠키에 있는 데이터 가져오기
				for(Cookie c : cookies) {
					if(c.getName().equals("exp")) {
						exp = c.getValue();
						break;
					}
				}			
			}

			resp.setCharacterEncoding("UTF-8");
			resp.setContentType("text/html; charset=UTF-8");
			PrintWriter out = resp.getWriter();

			out.write("<!DOCTYPE html>"); 
			out.write("<html>");
			out.write("		<head>");
			out.write("		<meta charset=\"UTF-8\">");
			out.write("		<title>Insert title here</title>");
			out.write("	<style type=\"text/css\">");
			out.write("	input{");
			out.write("		width: 50px;");
			out.write("		height: 50px;");
			out.write("	}");
			out.write("	.output {");
			out.write("			height: 50px;");
			out.write("		background : #e9e9e9;");
			out.write("		font-size: 24px;");
			out.write("		font-weight: bold;");
			out.write("		text-align: right;");
			out.write("		padding: 0px 5px;");
			out.write("	}");
			out.write("	</style>");
			out.write("	</head>");
			out.write("	<body>");
			out.write("		<form action=\"calc2\" method=\"post\"> ");
			out.write("			<table>");
			out.write("				<tr>");
			out.printf("					<td class =\"output\" colspan =4>%s</td>", exp);								
			out.write("				</tr>");
			out.write("				<tr>");
			out.write("					<td><input type=\"submit\" name = \"operator\" value = \"CE\"></td>");			
			out.write("					<td><input type=\"submit\" name = \"operator\" value = \"C\"></td>");			
			out.write("					<td><input type=\"submit\" name = \"operator\" value = \"BS\"></td>");				
			out.write("				<td><input type=\"submit\" name = \"operator\" value = \"/\"></td>");						
			out.write("			</tr>");
			out.write("			<tr>");
			out.write("				<td><input type=\"submit\" name = \"value\" value = \"7\"></td>");				
			out.write("				<td><input type=\"submit\" name = \"value\" value = \"8\"></td>");				
			out.write("					<td><input type=\"submit\" name = \"value\" value = \"9\"></td>");				
			out.write("				<td><input type=\"submit\" name = \"operator\" value = \"*\"></td>");						
			out.write("			</tr>");
			out.write("			<tr>");
			out.write("				<td><input type=\"submit\" name = \"value\" value = \"4\"></td>");				
			out.write("				<td><input type=\"submit\" name = \"value\" value = \"5\"></td>");				
			out.write("					<td><input type=\"submit\" name = \"value\" value = \"6\"></td>");				
			out.write("					<td><input type=\"submit\" name = \"operator\" value = \"-\"></td>");						
			out.write("				</tr>");
			out.write("				<tr>");
			out.write("					<td><input type=\"submit\" name = \"value\" value = \"1\"></td>");				
			out.write("					<td><input type=\"submit\" name = \"value\" value = \"2\"></td>");				
			out.write("					<td><input type=\"submit\" name = \"value\" value = \"3\"></td>");				
			out.write("				<td><input type=\"submit\" name = \"operator\" value = \"+\"></td>");						
			out.write("				</tr>");
			out.write("				<tr>");
			out.write("					<td></td>");				
			out.write("					<td><input type=\"submit\" name = \"value\" value = \"0\"></td>");				
			out.write("				<td><input type=\"submit\" name = \"dot\" value = \".\"></td>");				
			out.write("				<td><input type=\"submit\" name = \"operator\" value = \"=\"></td>");						
			out.write("				</tr>");				
			out.write("			</table>");
			out.write("		</form>");
			out.write("	</body>");
			out.write("	</html>");

		}

	}

	@WebServlet("/calc2")
	public class Calc2 extends HttpServlet  {

		@Override
		protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

			Cookie[] cookies = req.getCookies();

			String value = req.getParameter("value"); // 배열로 받는 경우
			String operator = req.getParameter("operator");
			String dot = req.getParameter("dot");

			String exp = "";
			if(cookies != null)
				for(Cookie c : cookies) {
					if(c.getName().equals("exp")) {
						exp = c.getValue();
						break;
					}
				}

			if(operator != null && operator.equals("=")) { // operator이 null이면 문자열 비교 불가 에러

			} else {
				exp += (value == null)?"":value;
				exp += (operator == null)?"":operator;
				exp += (dot == null)?"":dot;			
			}


			Cookie expCookie = new Cookie("exp", exp);
			if(operator != null && operator.equals("C")) {
				expCookie.setMaxAge(0);   // 쿠키 만료를 0초를 하면 삭제랑 같음
			}
			resp.addCookie(expCookie);  
			resp.sendRedirect("/calcpage");
		}

	}

## 13. GET과 POST에 특화된 서비스 함수

### 13.1 getMethod를 이용하는 방법
get 요청과 post 요청을 구분하는 방법 2가지

	@Override
	protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

		if(req.getMethod().equals("GET")) { // get 이란 문자가 왔다는걸 대문자로 사용함
			System.out.println("GET 요청이 왔습니다.");
		} else if (req.getMethod().equals("POST")) {
			System.out.println("POST 요청이 왔습니다.");	
		}
		super.service(req, resp);
	}

### 13.2 service함수를 이용하는 방법

부모가 가지고 있는 service함수는 사용자가 요청을 하면 doGet이나 doPost를 실행하고 이 두 함수가 오버라이드가 안 돼 있으면 오류가 난다.

service를 이용하기 위해서는 doGET()과 doPOST를 오버라이드 해줘야한다.

doGet이나 doPost전에 실행할 문장이 있으면 service를 오버라이드 해서 코드를 만들고 <br>
doGet이나 doPost만 필요하면 service를 오버라이드 안해도 자동으로 호출된다.

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("doGet 메소드가 호출 되었습니다.");	
	}
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("doPOST 메소드가 호출 되었습니다.");	
	}

### 13.3 calculator get, post 합치기

1. 같은 페이지를 사용하므로 여기서 생성된 쿠리를 다른 페이지에서 쿠키를 사용 못하도록 설정 가능

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		Cookie[] cookies = req.getCookies(); 
		String exp = "0";
		
		if(cookies != null) {
			for(Cookie c : cookies) {
				if(c.getName().equals("exp")) {	
					exp = c.getValue();
					break;
				} 
			}
		}
			
		resp.setCharacterEncoding("UTF-8");
		resp.setContentType("text/html; charset=UTF-8");
		PrintWriter out = resp.getWriter();
		
		out.write("<!DOCTYPE html>");
		out.write("<html>");
		out.write("		<head>");
		out.write("		<meta charset=\"UTF-8\">");
		out.write("		<title>Insert title here</title>");
		out.write("	<style type=\"text/css\">");
		out.write("	input{");
		out.write("		width: 50px;");
		out.write("		height: 50px;");
		out.write("	}");
		out.write("	.output {");
		out.write("			height: 50px;");
		out.write("		background : #e9e9e9;");
		out.write("		font-size: 24px;");
		out.write("		font-weight: bold;");
		out.write("		text-align: right;");
		out.write("		padding: 0px 5px;");
		out.write("	}");
		out.write("	</style>");
		out.write("	</head>");
		out.write("	<body>");
		out.write("		<form method=\"post\"> "); // get과 post를 처리하는 페이지가 같기 때문에 action 생략 가능
		out.write("			<table>");
		out.write("				<tr>");
		out.printf("					<td class =\"output\" colspan =4>%s</td>",exp);								
		out.write("				</tr>");
		out.write("				<tr>");
		out.write("					<td><input type=\"submit\" name = \"operator\" value = \"CE\"></td>");			
		out.write("					<td><input type=\"submit\" name = \"operator\" value = \"C\"></td>");			
		out.write("					<td><input type=\"submit\" name = \"operator\" value = \"BS\"></td>");				
		out.write("				<td><input type=\"submit\" name = \"operator\" value = \"/\"></td>");						
		out.write("			</tr>");
		out.write("			<tr>");
		out.write("				<td><input type=\"submit\" name = \"value\" value = \"7\"></td>");				
		out.write("				<td><input type=\"submit\" name = \"value\" value = \"8\"></td>");				
		out.write("					<td><input type=\"submit\" name = \"value\" value = \"9\"></td>");				
		out.write("				<td><input type=\"submit\" name = \"operator\" value = \"*\"></td>");						
		out.write("			</tr>");
		out.write("			<tr>");
		out.write("				<td><input type=\"submit\" name = \"value\" value = \"4\"></td>");				
		out.write("				<td><input type=\"submit\" name = \"value\" value = \"5\"></td>");				
		out.write("					<td><input type=\"submit\" name = \"value\" value = \"6\"></td>");				
		out.write("					<td><input type=\"submit\" name = \"operator\" value = \"-\"></td>");						
		out.write("				</tr>");
		out.write("				<tr>");
		out.write("					<td><input type=\"submit\" name = \"value\" value = \"1\"></td>");				
		out.write("					<td><input type=\"submit\" name = \"value\" value = \"2\"></td>");				
		out.write("					<td><input type=\"submit\" name = \"value\" value = \"3\"></td>");				
		out.write("				<td><input type=\"submit\" name = \"operator\" value = \"+\"></td>");						
		out.write("				</tr>");
		out.write("				<tr>");
		out.write("					<td></td>");				
		out.write("					<td><input type=\"submit\" name = \"value\" value = \"0\"></td>");				
		out.write("				<td><input type=\"submit\" name = \"dot\" value = \".\"></td>");				
		out.write("				<td><input type=\"submit\" name = \"operator\" value = \"=\"></td>");						
		out.write("				</tr>");				
		out.write("			</table>");
		out.write("		</form>");
		out.write("	</body>");
		out.write("	</html>");	
	}
	
	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		Cookie[] cookies = req.getCookies();
		
		String value = req.getParameter("value"); // 배열로 받는 경우
		String operator = req.getParameter("operator");
		String dot = req.getParameter("dot");
		
		String exp = "";
		
		
		if(cookies != null)
			for(Cookie c : cookies) {
				if(c.getName().equals("exp")) {
					exp = c.getValue();
					break;
				}
			}
		
		if(operator != null && operator.equals("=")) {
			
		} else {
			exp += (value == null)?"":value;
			exp += (operator == null)?"":operator;
			exp += (dot == null)?"":dot;			
		}
		
		
		Cookie expCookie = new Cookie("exp", exp);
		
		if(operator != null && operator.equals("C")) {
			expCookie.setMaxAge(0);
				
		}
		expCookie.setPath("/calculator"); // 같은 페이지를 사용하므로 다른 페이지에서 쿠키를 사용 못하도록 설정 가능
		resp.addCookie(expCookie);
		resp.sendRedirect("calculator"); // 자기가 자기를 호출하면 get요청
	}
	
## 14. JSP

JSP 란 JavaServer Pages 의 약자이며

HTML 코드에 JAVA 코드를 넣어 동적웹페이지를 생성하는 웹어플리케이션 도구이다.

JSP 가 실행되면 자바 서블릿(Servlet) 으로 변환되며 웹 어플리케이션 서버에서 동작되면서 필요한 기능을 수행하고

그렇게 생성된 데이터를 웹페이지와 함께 클라이언트로 응답한다.

### 14.1 코드블록

![image](https://user-images.githubusercontent.com/81665608/140748039-8c99813d-4128-4547-b962-6dbe5dce1e11.png)

![image](https://user-images.githubusercontent.com/81665608/140748143-b8ebe105-e17f-44e8-8142-d2442fb1f527.png)

![image](https://user-images.githubusercontent.com/81665608/140748297-86d355d2-46ee-426b-ae9b-800ebaee6f9a.png)

![image](https://user-images.githubusercontent.com/81665608/140748376-03f3b2f1-8de7-44be-b263-146aafa79fe2.png)

![image](https://user-images.githubusercontent.com/81665608/140748474-62b40186-d668-439c-baf8-bdd6fa2077c5.png)

### 14.2 코드블록의 내장 객체

제스퍼가 만들어낸 서블릿안에 미리 선언한 객체

![image](https://user-images.githubusercontent.com/81665608/140749293-103858a6-e429-4f16-bf2f-1cde374fe869.png)

![image](https://user-images.githubusercontent.com/81665608/140749341-c0e6a89b-8b0d-480f-aacd-fbdb03a1ed0f.png)

![image](https://user-images.githubusercontent.com/81665608/140749534-a0bcfd8f-ea94-4511-86f7-21039c6f9caf.png)

![image](https://user-images.githubusercontent.com/81665608/140749611-25234bb8-f08c-4feb-913a-1aa4303ebc4b.png)


### 14.3 JSP로 만드는 Hello 서블릿

	<%@ page language="java" contentType="text/html; charset=UTF-8"
		pageEncoding="UTF-8"%>
	<%

	String cnt_ = request.getParameter("cnt");

	int cnt = 10;

	if(cnt_ != null && !cnt_.equals("")) {
		cnt = Integer.parseInt(cnt_);
	}

	%>
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	</head>
	<body>
		<%for(int i = 0 ; i < cnt; i++) {%>
		안녕안녕 <br>
		<% } %>
	</body>
	</html>
	
## 15. JSP MVC model

### 15.1 JSP MVC model1

![image](https://user-images.githubusercontent.com/81665608/140857558-8ab335a5-969c-49f0-95bc-585311ed5218.png)

	<%	
		int num = 0;
		String num_ = request.getParameter("n");
		if(num_ != null && !num_.equals("")){
			num = Integer.parseInt(num_);
		}
		String result;

		if(num % 2 != 0){
			result = "홀수"	;
		} else {
			result = "짝수";
		}
	%>

	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	</head>
	<body>
		<%=result %> 입니다.
	</body>
	</html>

	
### 15.2 JSP MVC model2
![image](https://user-images.githubusercontent.com/81665608/140864040-a05b37e5-1de5-4f9a-9d91-57f586a0ac37.png)
	
	
	@WebServlet("/spag")
	public class Spag extends HttpServlet{

		@Override
		protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

			int num = 0;
			String num_ = req.getParameter("n");
			if(num_ != null && !num_.equals("")){
				num = Integer.parseInt(num_);
			}
			String result;

			if(num % 2 != 0){
				result = "홀수";
			} else {
				result = "짝수";
			}

			req.setAttribute("result", result ); // request - forward 관계에서 사용할 수 있는 저장소 

			//redirect - 현재 작업한 내용과 상관없이 새로운 요청
			//forward - 현재 작업한 내용을 이어갈 수 있도록 공유

			RequestDispatcher dispatcher =  req.getRequestDispatcher("/spag.jsp"); // 요청할곳 경로 설정
			dispatcher.forward(req, resp);
		}

	}
	
	
	<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
 
 
	<!DOCTYPE html>
	<html>
	<head>
	<meta charset="UTF-8">
	<title>Insert title here</title>
	</head>
	<body>
		<%=request.getAttribute("result") %> 입니다.
	</body>
	</html>
	
### 15.3 View를 위한 데이터 추출 표현식 EL

![image](https://user-images.githubusercontent.com/81665608/140867954-5503fa75-1d31-4dcc-82ba-a80b99650e9e.png)

![image](https://user-images.githubusercontent.com/81665608/140868474-07ffb01f-8629-42de-bb8b-3126fd3cdd63.png)

### 15.4 EL의 저장소

만약 저장소에 저장된 키의 이름이 같은 경우 우선순위

![image](https://user-images.githubusercontent.com/81665608/140869626-2e9d536e-9220-4b15-b8da-99bef7ff88da.png)

어떤 저장소를 특정하고 싶은경우 ${sessionScope.cnt}

### 15.5 EL을 사용할 수 있는 여러가지 객체

![image](https://user-images.githubusercontent.com/81665608/140870422-3634aa99-5491-41d1-9009-6a6523ee3501.png)

pageContext 같은 경우 함수 호출 시 ${pageContext.request.method }

### 15.6 EL 연산자

![image](https://user-images.githubusercontent.com/81665608/140872593-6d2ee334-fa0b-40af-a57c-5bf34cc5a60d.png)

empty - null 이거나 값이 비어있으면 참 ${empty param.n}


## 16. JSP를 이용한 웹 프로그래밍

### 16.1 JDBC를 이용해 글 목록 구현하기

	<table class="table">
		<thead>
			<tr>
				<th class="w60">번호</th>
				<th class="expand">제목</th>
				<th class="w100">작성자</th>
				<th class="w100">작성일</th>
				<th class="w60">조회수</th>
			</tr>
		</thead>
		<tbody>

		<% while(rs.next()){ %>

		<tr>
			<td><%=rs.getInt("ID") %></td>
			<td class="title indent text-align-left"><a href="detail.html"><%=rs.getString("TITLE")%></a></td>
			<td><%= rs.getString("WRITER_ID") %></td>
			<td>
				<%=rs.getDate("REGDATE") %>		
			</td>
			<td><%=rs.getInt("HIT") %></td>
		</tr>
		<%} %>
		</tbody>
	</table>
	
### 16.2 자세한 페이지 구현하기

	<table class="table">
		<tbody>
			<tr>
				<th>제목</th>
				<td class="text-align-left text-indent text-strong text-orange" colspan="3"><%=rs.getString("TITLE") %></td>
			</tr>
			<tr>
				<th>작성일</th>
				<td class="text-align-left text-indent" colspan="3"><%= rs.getDate("REGDATE") %>	</td>
			</tr>
			<tr>
				<th>작성자</th>
				<td><%=rs.getString("WRITER_ID") %></td>
				<th>조회수</th>
				<td><%= rs.getInt("HIT") %></td>
			</tr>
			<tr>
				<th>첨부파일</th>
				<td colspan="3"></td>
			</tr>
			<tr class="content">
				<td colspan="4"><%= rs.getString("CONTENT") %></td>
			</tr>
		</tbody>
	</table>

### 16.3 자세한 페이지 MVC model1로 변경하기

	<%	
	int id = Integer.parseInt(request.getParameter("id"));
	String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
	String sql = "SELECT * FROM NOTICE WHERE ID = ?";

	Class.forName("oracle.jdbc.driver.OracleDriver");
	Connection con = DriverManager.getConnection(url, "", "");
	PreparedStatement st = con.prepareStatement(sql);
	st.setInt(1, id);

	ResultSet rs = st.executeQuery(); 

	rs.next();

	String title = rs.getString("TITLE");
	Date regdate = rs.getDate("REGDATE");
	String writerId = rs.getString("WRITER_ID");
	int hit = rs.getInt("HIT"); 
	String content = rs.getString("CONTENT"); 

	rs.close();
	st.close();
	con.close();               		
	%>
	
	<table class="table">
		<tbody>
			<tr>
				<th>제목</th>
				<td class="text-align-left text-indent text-strong text-orange" colspan="3"><%=title %></td>
			</tr>
			<tr>
				<th>작성일</th>
				<td class="text-align-left text-indent" colspan="3"><%=regdate %></td>
			</tr>
			<tr>
				<th>작성자</th>
				<td><%=writerId %></td>
				<th>조회수</th>
				<td><%=hit %></td>
			</tr>
			<tr>
				<th>첨부파일</th>
				<td colspan="3"></td>
			</tr>
			<tr class="content">
				<td colspan="4"><%=content %></td>
			</tr>
		</tbody>
	</table>
	
### 16.4 자세한 페이지 MVC model2로 변경하기

![image](https://user-images.githubusercontent.com/81665608/140907729-d70b2d04-2212-458c-9fa1-4adf22c2ee98.png)

protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		int id = Integer.parseInt(request.getParameter("id"));
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "SELECT * FROM NOTICE WHERE ID = ?";

		try {			
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url, "ohji", "tkwl1414");
			PreparedStatement st = con.prepareStatement(sql);
			st.setInt(1, id);

			ResultSet rs = st.executeQuery(); 
				
			rs.next();
				
			String title = rs.getString("TITLE");
			Date regdate = rs.getDate("REGDATE");
			String writerId = rs.getString("WRITER_ID");
			int hit = rs.getInt("HIT"); 
			String content = rs.getString("CONTENT"); 
			 
			request.setAttribute("title", title);
			request.setAttribute("regdate", regdate);
			request.setAttribute("writerId", writerId);
			request.setAttribute("hit", hit);
			request.setAttribute("content", content);
			
			rs.close();
			st.close();
			con.close();
			
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		} catch (SQLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		//redirect  
		//forward
		RequestDispatcher dispatcher =  request.getRequestDispatcher("/notice/detail.jsp");
		dispatcher.forward(request, response);	
	}

### 16.5 데이터의 구조화

데이터를 하나의 관련있는 객체로 만들어서 사용

![image](https://user-images.githubusercontent.com/81665608/140916119-e49e8086-213b-4d92-8a52-184c2269154a.png)


### 16.6 게시판 목록 MVC model2로 변경하기

	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	
		String url = "jdbc:oracle:thin:@localhost:1521/xepdb1";
		String sql = "SELECT * FROM NOTICE";

		List<Notice> list = new ArrayList<Notice>(); // 게시판 목록이 많으니 리스트 객체 생성

		try {
			Class.forName("oracle.jdbc.driver.OracleDriver");
			Connection con = DriverManager.getConnection(url, "ohji", "tkwl1414");
			Statement st = con.createStatement();
			ResultSet rs = st.executeQuery(sql);

			while(rs.next()){
				int id = rs.getInt("ID");
				String title = rs.getString("TITLE");
				Date regdate = rs.getDate("REGDATE");
				String writerId = rs.getString("WRITER_ID");
				int hit = rs.getInt("HIT"); 
				String content = rs.getString("CONTENT");

				Notice notice = new Notice(
						id,
						title,
						regdate,
						writerId,
						hit,
						content			
					);
				list.add(notice);
			}	

			rs.close();
			st.close();
			con.close();

		} catch (ClassNotFoundException e) {		
			e.printStackTrace();
		} catch (SQLException e) {
			e.printStackTrace();
		}

		request.setAttribute("list", list);

		request
		.getRequestDispatcher("/WEB-INF/view/notice/list.jsp")
		.forward(request, response);

	}	
	
	<tbody>
	<%-- 
	<%
	List<Notice> list = (List<Notice>)request.getAttribute("list");
	for(Notice n  : list){ // 지역변수만 가능하니 list를 빼줌
		pageContext.setAttribute("n", n); // EL은 지역변수 말고 저장소만 사용가능 그래서 다시 저장소에 넣어줌
	%>			
	 --%>
	 <c:forEach var = "n" items="${list}"> // forEach가 위에 과정을 처리해줌 JSTL 라이브러리 이용
	<tr>
		<td>${n.id})</td>
		<td class="title indent text-align-left"><a href="detail?id=${n.id }">${n.title }</a></td>
		<td>${n.writerId }</td>
		<td>${n.regdate }</td>
		<td>${n.hit }</td>
	</tr>
	</c:forEach>
	<%-- <%} %> --%>
	</tbody>
	
	

### 16.7 View 페이지 은닉하기

view 페이지를 사용자가 요청할 수 있는데 디렉토리에 있으므로 MVC model2를 이용해서 만들었다면 View 페이지는 숨겨야한다. 왜냐하면 Controller 페이지 부터 시작하기 때문이다.

사용자가 접근할 수 없는 WEB_INF 폴더에 View 페이지가 담긴 폴더를 옮기고 Cotroller 페이지에서 아래와 같이 셋팅하면 된다.

	request.getRequestDispatcher("/WEB-INF/view/notice/detail.jsp");

### 16.8 JSTL

https://mvnrepository.com/artifact/javax.servlet/jstl/1.2 에서 .jar 파일 다운로드


#### 16..1 forEach

- 속성
1. items : forEach가 순회할 Collection 개체를 지정한다. 
2. begin : 반복문의 시작값을 설정한다. (0시작)
3. end : 반복문의 종료값을 설정한다. 
4. step : 반복문의 증가값을 설정한다. 
5. var : 반복문의 순회시 해당하는 값을 담을 변수를 설정한다. 
6. varStatus : 변수의 상태를 담을 변수를 설정한다. 

- varStatus : 

		<c:forEach items="${LIST}" var="item" varStatus="st"
		${status.current} : 현재 아이템 
		${status.index} : 0부터의 순서
		${status.count} : 1부터의 순서
		${status.first} : 현재 루프가 처음인지 반환
		${status.last} : 현재 루프가 마지막인지 반환
		${status.begin} : 시작값
		${status.end} : 끝값
		${status.step} : 증가값























