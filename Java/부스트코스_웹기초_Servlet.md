#### 백엔드 다시 보기

##### 웹 서버

소프트웨어 혹은 웹 서버 소프트웨어가 동작하는 컴퓨터
클라이언트가 요청하는 리소스를 전달하는 것
다른 말로 HTTP Server라고도 부른다.
Apach 웹 서버가 대표적이다.
Nginx는 차세대 웹서버로 더 적은 자원으로 더 빠르게 데이터를 서비스하는 것을 목적으로 만들어졌다. 개발의 모든 목적이 높은 성능에 맞춰져 있으며, 잘 사용하지 않는 기능은 과감하게 제외하였다.
( 2020년 현재, nginx의 점유율이 Apache를 넘어섬. )

https://news.netcraft.com/archives/2020/06/25/june-2020-web-server-survey.html

##### WAS, 미들웨어

클라이언트 쪽에 비즈니스 로직이 많을 경우, 클라이언트 관리 (배포 등)로 인해 비용이 많이 발생하는 문제가 있으며, 보안 상에도 좋지 못하다. 비즈니스 로직을 클라이언트와 DBMS 사이의 미들웨어 서버에서 동작하도록 함으로써 클라이언트는 입력과 출력만 담당하도록 한다.

WAS는 미들웨어의 일종으로 웹 클라이언트의 요청 중, 웹 애플리케이션이 동작하도록 지원하는 목적을 가진다. 웹 애플리케이션을 실행하기 위한 OS에 비유할 수 있다.
Apache Tomcat이 대표적인 WAS이다.

##### Java Web Application

WAS가 OS이면, Java Web Application은 OS에서 실행되는 프로그램에 비유할 수 있다.
즉, WAS에 deploy되어 동작하는 Applicaion이다.
Java Web Applicaion에는 HTML, CSS, 이미지, Java로 작성된 클래스( Servlet 포함, package, 인터페이스 등 ), 각종 설정 파일 등이 포함된다.

[![img](https://cphinf.pstatic.net/mooc/20180124_133/15167752967943AqfC_PNG/1_5_1_____.PNG?type=w760)](https://www.edwith.org/boostcourse-web/lecture/16686/#)

- WAS위에서 동작하기 때문에 일정한 구조를 따라야한다.
- web.xml은 웹 어플리케이션에 대한 정보를 다 가지고 있는 파일이다.

> Dynamic Web Project를 만들면서 Target runtime은 실행 대상이 되는 WAS(ex.톰캣)가 되고, 이 WAS 위에서 실행되는 Dynamic web module은 Servlet이다. 이 Servlet이 애플리케이션의 동적인 처리를 담당한다.
>
> Source를 컴파일된 파일들이 저장되는 것이 output foler이며, default로 build\classes로 되어있다.

##### Servlet

Java Web Application의 구성 요소 중, 동적인 처리를 하는 프로그램이다.
Servlet은 HTTPServlet 클래스를 상속받아야 하며, Servlet과 JSP로부터 최상의 결과를 얻으려면, 웹 페이지를 개발할 때 이 두가지(JSP, Servlet)를 조화롭게 사용해야 한다.
예를 들어, 웹페이지를 구성하는 화면(HTML)은 JSP로 표현하고, 복잡한 프로그래밍은 Servlet으로 구현한다.

> HTTPServlet 클래스에는 Servlet으로 동작할 수 있는 부분이 구현되어 있으며, 이 클래스를 상속 받음으로써 편하게 Servlet으로 동작하는 클래스를 만들 수 있다.
>
> 이클립스에서 Servlet이라는 파일을 생성할 수 있는데, 사실 그냥 Java 파일이며 일종의 템플릿처럼 작성해야 할 부분들을 몇가지 설정을 통해서 간단하게 만들어주는 마법사 같은 기능이라 볼 수 있다.

현재 프로젝트에서 웹을 개발할 때 Servlet을 직접 써서 개발하지는 않는다. 조금 더 편하게 사용할 수 있게 도와주는 다양한 프레임워크를 사용하여 개발하는 경우가 더 많다.

Servlet 버전(Dynamic web module version)에 따라 3.0 이상의 버전은 web.xml이 아니라 annotaion을 사용한다.

```java
@WebServlet("/ten")
public class Tenservlet extends HttpServlet { ... }
```

3.0 미만에서는 web.xml에 직접 Servlet을 등록해준다.

```xml
<servlet>
	<description></description>
    <display-name>TenServlet</display-name>
    <servlet-name>TenServlet</servlet-name>
    <servlet-class>exam.TenServlet</servlet-class>
</servlet>
<servlet-mapping>
	<servlet-name>TenServlet</servlet-name>
    <url-pattern>/ten</url-pattern>
</servlet-mapping>
```

##### Servlet Lifecycle

1. 클라이언트 URL 입력. 요청이 들어옴.
2. URL에 일치하는 Servlet을 찾음.
3. 해당 Servlet가 메모리에 없으면 메모리에 올려서 객체를 생성함. (이때 생성자 호출)
4. **init()** 호출.
5. **service(request, respnse)** 호출. 이후 요청이 있을 때마다 **service()** 호출
   ( 다른 브라우저에서 요청이 있을지라도 서버에 Servlet 객체는 하나만 생성되기 때문에 **service()** 만 호출된다. )
6. **destroy()** 는 웹 어플리케이션이 갱신되거나, WAS가 종료될 때 호출된다.

[![img](https://cphinf.pstatic.net/mooc/20180124_22/1516782982944xjogH_PNG/1_5_3_ServletLifcycle.PNG?type=w760)](https://www.edwith.org/boostcourse-web/lecture/16688/#)

###### service(request, response) 메서드

HttpServlet의 service 메서드는 **템플릿 메서드 패턴으로 구현**

- 클라이언트의 요청이 **GET**인 경우, 자신이 가지고 있는 **doGet(requrest, response)** 메서드를 호출
- 클라이언트의 요청이 **POST**인 경우, 자신이 가지고 있는 **doPost(request, response)** 메서드를 호출
- 부모 클래스인 HttpServlet에서 service 클래스를 구현해놓았고, 이 부분을 자식 클래스(servlet 클래스)에서 따로 구현하지 않으면, 부보 클래스의 service 클래스가 실행이 된다.
- HttpServlet의 service 클래스는 요청 방식에 따라 doGet, doPost가 실행이 되도록 구현이 되어있다.
- 즉, service는 이미 세분화 되어서, service만 실행되는 것이 아니라 요청에 따라 이미 정해진 템플릿 메서드들이 실행된다.

##### HttpServletRequst, HttpServletResponse

[![img](https://cphinf.pstatic.net/mooc/20180124_79/15167843899250uB2H_PNG/1_5_4_request_response.PNG?type=w760) ](https://www.edwith.org/boostcourse-web/lecture/16689/#)

WAS는 웹 브라우저로부터 Servlet 요청을 받으면,

- 요청할 때 가지고 있는 정보를 **HttpServletRequest **객체를 생성하여 저장
- 웹 브라우저에게 응답을 보낼 때 사용하기 위하여 **HttpServletResponse** 객체를 생성

- 생성된 HttpServletRequest, HttpServletResponse 객체를 Servlet의 **service()에 파라미터로 전달**

###### HttpServletRequest

- http 프로토콜의 request 정보를 servlet에게 전달하기 위한 목적으로 사용
- 헤더 정보, 파라미터, 쿠키, URI, URL 등의 정보를 읽어들이는 메소드를 가지고 있다.
- Body의 Stream을 읽어들이는 메소드를 가지고 있다.

###### HttpServletResponse

- WAS는 어떤 클라이언트가 요청을 보냈는지 알고 있고, 해당 클라이언트에게 응답을 보내기 위한 HttpServletResponse 객체를 생성하여 Servlet에게 전달
- Servlet은 해당 객체를 이용하여 content type, 응답코드, 응답 메세지 등을 전송

###### HttpServletRequest 객체가 가지고 있는 값

- uri : 도메인과 포트번호 이하의 값
- url: 요청주소 전체
- contentPath: 웹 어플리케이션과 연결된 이름
- remoteAddr: 클라이언트의 ip 주소 값
