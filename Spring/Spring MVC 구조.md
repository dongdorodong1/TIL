## Spring MVC 구조

![MVC구조](/image/SpringMVC구조.png)

> ### 동작순서<br>
> 1. 핸들러 조회: 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회
> 2. 핸들러 어댑터 조회: 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.
> 3. 핸들러 어댑터 실행: 핸들러 어댑터를 실행한다.
> 4. 핸들러 실행: 핸들러 어댑터가 실제 핸들러를 실행한다.
> 5. ModelAndView 반환: 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환해서 반환한다.
> 6. ViewResolver 호출: 뷰 리졸버를 찾고 실행한다.
> 7. View반환: 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.
> 8. 뷰 렌더링: 뷰를 통해서 뷰를 렌더링 한다.

<br>
내부 구조를 파악하는 것은 쉽지 않다. 동작을 알아두는 것은 문제가 발생했을때 어떤 부분에서 문제가  발생했는지 파악하고 해결할 수 있다.

핸들러 매핑 순서
1. RequestMapping을 통해 찾는다.
2. 스프링 빈의 이름으로 핸들러를 찾는다.

핸들러 어댑터 순서
1. RequestMappingHandlerAdapter : 애노테이션 기반의 컨트롤러인 @RequestMapping에서사용
2. HttpRequestHandlerAdapter : HttpRequestHandler 처리
3. SimpleControllerHandlerAdapter : Controller 인터페이스(애노테이션X, 과거에 사용) 
처리


