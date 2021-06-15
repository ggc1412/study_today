### Spring 설정

#### filter & interceptor

둘 다 우선 순위에 의하여 체이닝 된다. 
interceptor가 filter와 다른 점은 Request, Response 뿐만아니라 여러가지 상황에서 처리가 가능하며 Interceptor 역시 Bean이므로 다른 Bean을 DI처리가 가능하다는 것이다.

![img](https://t1.daumcdn.net/cfile/tistory/9954C83F5AC0B5A31C)

![SpringMVCRequestLifecycle](https://stargatex.files.wordpress.com/2015/12/springmvcrequestlifecycle.jpg?w=800)



#### 이벤트 리스너, 이벤트 핸들러

Servlet/JSP에서 리스너는 웹어플리케이션의 시작이나, 종료, 특정 객체의 생성, 소멸과 같은 것을 다루는 이벤트 리스너, 핸들러가 있다.

ServletContext

- ServletContextListener - 웹 어플리케이션의 시작, 종료 이벤트에 대한 이벤트 리스너이다.
- ServletContextAttributeListener - ServletContext에 attribute를 추가하거나 제거, 수정됐을 때에 대한 이벤트 리스너입니다

HttpSession

- HttpSessionListener - HTTP 세선의 시작, 종료 이벤트에 대한 이벤트 리스너이다.
- HttpSessionAttributeListener - HttpSession에 attribute를 추가하거나 제거, 수정했을 때에 대한 이벤트 리스너이다.

ServletRequest

- ServletRequestListener - 클라이언트로부터의 요청으로 인한 ServletRequest 생성과 응답 이후 ServletRequest 제거에 대한 이벤트 리스너이다.
- ServletRequestAttributeListener - ServletRequest에 attribute를 추가하거나 제거, 수정했을 때에 대한 이벤트 리스너이다. 



