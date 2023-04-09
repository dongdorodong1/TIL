# ViewResolver


### 스프링 부트가 자동 등록하는 뷰 리졸버
- 1 = BeanNameViewResolver : 빈 이름으로 뷰를 찾아서 반환한다. (예: 엑셀 파일 생성기능에 사용)
- 2 = InternalResourceViewResolver : JSP를 처리할 수 있는 뷰를 반환한다.
  <br>

### 뷰 리졸버 - InternalResourceViewResolver
스프링 부트는 InternalResourceViewResolver 라는 뷰 리졸버를 자동으로 등록하는데, 이때
application.properties 에 등록한 spring.mvc.view.prefix , spring.mvc.view.suffix 설정 정보를 사용해서 등록한다.<br>
*템플릿 엔진마다 리졸버가 다름
<br>
> ### 뷰 리졸버 동작 방식
1. 핸들러 어댑터 호출<br>
핸들러 어댑터를 통해 new-form 이라는 논리 뷰 이름을 획득한다.<br><br>
1. ViewResolver 호출<br>
new-form 이라는 뷰 이름으로 viewResolver를 순서대로 호출한다.<br>
BeanNameViewResolver 는 new-form 이라는 이름의 스프링 빈으로 등록된 뷰를 찾아야 하는데 없다.
InternalResourceViewResolver 가 호출된다.<br>

1. InternalResourceViewResolver<br>
이 뷰 리졸버는 InternalResourceView 를 반환한다.<br>

1. 뷰 - InternalResourceView<br>
InternalResourceView 는 JSP처럼 포워드 forward() 를 호출해서 처리할 수 있는 경우에 사용한다.

1. view.render()<br>
view.render() 가 호출되고 InternalResourceView 는 forward() 를 사용해서 JSP를 실행한다.



