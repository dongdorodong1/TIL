# Spring과 HTTP

<b>클라이언트에서 서버로 요청 데이터를 전달할 때는 주로 다음 3가지 방법을 사용한다.</b>

### 요청 파라미터(request parameter) 조회

>1. GET - 쿼리 파라미터
/url?username=hello&age=20
메시지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달<br>
예) 검색, 필터, 페이징등에서 많이 사용하는 방식<br><br>
>2. POST - HTML Form
content-type: application/x-www-form-urlencoded
메시지 바디에 쿼리 파리미터 형식으로 전달 username=hello&age=20<br>
예) 회원 가입, 상품 주문, HTML Form 사용<br><br>


1. HTTP message body에 데이터를 직접 담아서 요청
HTTP API에서 주로 사용, JSON, XML, TEXT
데이터 형식은 주로 JSON 사용
POST, PUT, PATCH

- HTTP 파라미터 이름이 변수 이름과 같으면 @RequestParam(name="xx") 생략 가능
- String , int , Integer 등의 단순 타입이면 @RequestParam 도 생략 가능<br>
*requestParam 조차 생략하는것은 모호할 수 있다(팀이 잘 쓴다면 생략)


## @ModelAttribute

- 스프링MVC는 @ModelAttribute 가 있으면 다음을 실행한다.
HelloData 객체를 생성한다.<br>
- 요청 파라미터의 이름으로 HelloData 객체의 프로퍼티를 찾는다. 그리고 해당 프로퍼티의 setter를 호출해서 파라미터의 값을 입력(바인딩) 한다.
    - 예) 파라미터 이름이 username 이면 setUsername() 메서드를 찾아서 호출하면서 값을 입력

> @ModelAttribute 는 생략할 수 있다.<br>
그런데 @RequestParam 도 생략할 수 있으니 혼란이 발생할 수 있다.

스프링은 해당 생략시 다음과 같은 규칙을 적용한다.
- String , int , Integer 같은 단순 타입 = @RequestParam
- 나머지 = @ModelAttribute (argument resolver 로 지정해둔 타입 외)

### @ModelAttribute - Model 추가
@ModelAttribute 는 중요한 한가지 기능이 더 있는데, 바로 모델(Model)에 @ModelAttribute 로
지정한 객체를 자동으로 넣어준다. 지금 코드를 보면 model.addAttribute("item", item) 가 주석처리
되어 있어도 잘 동작하는 것을 확인할 수 있다.  
모델에 데이터를 담을 때는 이름이 필요하다. 이름은 @ModelAttribute 에 지정한 name(value) 속성을
사용한다. 만약 다음과 같이 @ModelAttribute 의 이름을 다르게 지정하면 다른 이름으로 모델에
포함된다.  
<br>
@ModelAttribute("hello") Item item 이름을 hello 로 지정  
model.addAttribute("hello", item); 모델에 hello 이름으로 저장

주의  
@ModelAttribute 의 이름을 생략하면 모델에 저장될 때 클래스명을 사용한다. 이때 <b>클래스의 첫글자만
소문자로 변경</b>해서 등록한다.  
- 예) @ModelAttribute 클래스명 모델에 자동 추가되는 이름
    - Item > item
    - HelloWorld > helloWorld


<hr>
<br>

# HTTP 요청 메시지

## HTTP 요청 메시지 - 단순 텍스트
- 요청 파라미터와 다르게, HTTP 메시지 바디를 통해 데이터가 직접 넘어오는 경우는 @RequestParam, @ModelAttribute 를 사용할 수 없다.<br> (물론 HTML Form 형식으로 전달되는 경우는 요청 파라미터로
인정된다.

- ver.1
    - 스프링 MVC는 다음 파라미터를 지원한다.
InputStream(Reader): HTTP 요청 메시지 바디의 내용을 직접 조회
OutputStream(Writer): HTTP 응답 메시지의 바디에 직접 결과 출력

- ver.2
    - 스프링 MVC는 다음 파라미터를 지원한다.<br>
HttpEntity: HTTP header, body 정보를 편리하게 조회<br>
메시지 바디 정보를 직접 조회<br>
<b>요청 파라미터를 조회하는 기능과 관계 없음</b> 
@RequestParam X, @ModelAttribute X
<br>
    - HttpEntity는 응답에도 사용 가능
        - 메시지 바디 정보 직접 반환
        - 헤더 정보 포함 가능
        - view 조회X
> ### 참고
>스프링MVC 내부에서 HTTP 메시지 바디를 읽어서 문자나 객체로 변환해서 전달해주는데, 이때 HTTP 
메시지 컨버터( HttpMessageConverter )라는 기능을 사용한다.

### @RequestBody<br>
@RequestBody 를 사용하면 HTTP 메시지 바디 정보를 편리하게 조회할 수 있다.<br> 
참고로 헤더 정보가 필요하다면 HttpEntity 를 사용하거나 @RequestHeader 를 사용하면 된다. 이렇게 메시지 바디를 직접 조회하는 기능은 요청 파라미터를 조회하는 @RequestParam , @ModelAttribute 와는 전혀 관계가 없다.<br><br>
- 요청 파라미터 vs HTTP 메시지 바디<br>
    - 요청 파라미터를 조회하는 기능: @RequestParam , @ModelAttribute
    - HTTP 메시지 바디를 직접 조회하는 기능: @RequestBody
  
### @ResponseBody
@ResponseBody 를 사용하면 응답 결과를 HTTP 메시지 바디에 직접 담아서 전달할 수 있다.
물론 이 경우에도 view를 사용하지 않는다

```
* @RequestBody 생략 불가능(why? @ModelAttribute 가 적용되어 버림)
 * HttpMessageConverter 사용 -> MappingJackson2HttpMessageConverter (contenttype: application/json)
```

@ResponseBody 응답의 경우에도 @ResponseBody 를 사용하면 해당 객체를 HTTP 메시지 바디에 직접 넣어줄 수 있다.
물론 이 경우에도 HttpEntity 를 사용해도 된다.

- @RequestBody 요청
    - JSON 요청 HTTP 메시지 컨버터 객체
- @ResponseBody 응답
    - 객체 HTTP 메시지 컨버터 JSON 응답


### @RestController
@Controller 대신에 @RestController 애노테이션을 사용하면, 해당 컨트롤러에 모두 @ResponseBody 가 적용되는 효과가 있다. 따라서 뷰 템플릿을 사용하는 것이 아니라, HTTP 메시지 바디에 직접 데이터를 입력한다. 이름 그대로 Rest API(HTTP API)를 만들 때 사용하는 컨트롤러이다.<br>
참고로 @ResponseBody 는 클래스 레벨에 두면 전체 메서드에 적용되는데, @RestController
에노테이션 안에 @ResponseBody 가 적용되어 있다.





