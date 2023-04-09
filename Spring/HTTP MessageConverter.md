
# HTTP MessageConverter
> 클라이언트에 요청이 들어오면 데이터 타입에 따라 메시지 컨버터가 동작하여 서버에서 받고 처리할 수 있음 응답시도 동일
>

![MVC구조](/image/MessageConverter.png)

@ResponseBody 를 사용
HTTP의 BODY에 문자 내용을 직접 반환
viewResolver 대신에 HttpMessageConverter 가 동작
기본 문자처리: StringHttpMessageConverter
기본 객체처리: MappingJackson2HttpMessageConverter
byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음

### 주요 메시지 컨버터
- ByteArrayHttpMessageConverter
  - 클래스 타입: byte[], 미디어타입: \*/* 시 동작 
  - 요청 예) @RequestBody byte[] data
  - 응답 예) @ResponseBody return byte[] 쓰기 미디어타입 application/octet-stream
  <br><br>
- StringHttpMessageConverter
  - 클래스 타입: String, 미디어타입: \*/* 시 동작
  - 요청 예) @RequestBody String data
  - 응답 예) @ResponseBody return "ok" 쓰기 미디어타입 text/plain
    <br><br>
- MappingJackson2HttpMessageConverter
  - 클래스 타입: 객체 또는 hashamp, 미디어타입: application/json 시 동작
  - 요청 예) @RequestBody HelloData data
  - 응답 예) @ResponseBody return helloData 쓰기 미디어타입 application/json 관련
> \*\/\* : 아무 미디어타입이나 상관없다는 의미 
> <br>ex) text/plain , apllication/json

<hr>

### StringHttpMessageConverter

``` java
content-type: application/json

@RequestMapping
void hello(@ReqeustBody String data){}
```

1. byte[]? 아니야
2. String? mediatype check >json이네? > ok
   
   <br>
   
### MappingJackson2HttpMessageConverter
우선순위는 무조건 String이다.
``` java
content-type: application/json

@RequestMapping
void hello(@ReqeustBody HelloData data){}
```

1. byte[]? 아니야 > String? 아니야
2. String? mediatype check >json이네? > ok
<hr>

### ?

``` java
content-type: text/html

@RequestMapping
void hello(@ReqeustBody HelloData data){}
```

1. byte[]? 아니야 > String? 아니야 > json ok
2. String? mediatype check > text/html? > 탈락
<hr>


> 스프링 부트는 다양한 메시지 컨버터를 제공하는데, 대상 클래스 타입과 미디어 타입 둘을 체크해서
사용여부를 결정한다. 만약 만족하지 않으면 다음 메시지 컨버터로 우선순위가 넘어간다.

요청시 text/plain으로 보내고 서버에서 객체로 받으면 JsonConverter가 실행되지 않는다.

HTTP요청: @RequstBody, HttpEntity(RequestEntity)
HTTP응답: @ResponseBody, HttpEntity(ResponseEntity)

<br>

### HTTP 요청 데이터 읽기
- HTTP 요청이 오고, 컨트롤러에서 @RequestBody , HttpEntity 파라미터를 사용한다.
- 메시지 컨버터가 메시지를 읽을 수 있는지 확인하기 위해 canRead() 를 호출한다.
  - 대상 클래스 타입을 지원하는가.
    - 예) @RequestBody 의 대상 클래스 ( byte[] , String , HelloData )
  - HTTP 요청의 Content-Type 미디어 타입을 지원하는가.
    - 예) text/plain , application/json , */*
- canRead() 조건을 만족하면 read() 를 호출해서 객체 생성하고, 반환한다.
  
### HTTP 응답 데이터 생성
- 컨트롤러에서 @ResponseBody , HttpEntity 로 값이 반환된다. 
- 메시지 컨버터가 메시지를 쓸 수 있는지 확인하기 위해 canWrite() 를 호출한다.
  - 대상 클래스 타입을 지원하는가.
    - 예) return의 대상 클래스 ( byte[] , String , HelloData )
  - HTTP 요청의 Accept 미디어 타입을 지원하는가.(더 정확히는 @RequestMapping 의 produces )
    - 예) text/plain , application/json , */*
- canWrite() 조건을 만족하면 write() 를 호출해서 HTTP 응답 메시지 바디에 데이터를 생성한다.
<br><br>

## 요청 매핑 헨들러 어뎁터 구조
![MVC구조](/image/매핑어댑터.png)


### ArgumentResolver
애노테이션 기반의 컨트롤러는 매우 다양한 파라미터를 사용할 수 있었다.
HttpServletRequest , Model 은 물론이고, @RequestParam , @ModelAttribute 같은 애노테이션
그리고 @RequestBody , HttpEntity 같은 HTTP 메시지를 처리하는 부분까지 매우 큰 유연함을
보여주었다.
이렇게 파라미터를 유연하게 처리할 수 있는 이유가 바로 ArgumentResolver 덕분이다.
애노테이션 기반 컨트롤러를 처리하는 RequestMappingHandlerAdapter 는 바로 이
ArgumentResolver 를 호출해서 컨트롤러(핸들러)가 필요로 하는 다양한 파라미터의 값(객체)을 생성한다. 
그리고 이렇게 파리미터의 값이 모두 준비되면 컨트롤러를 호출하면서 값을 넘겨준다.
스프링은 30개가 넘는 ArgumentResolver 를 기본으로 제공한다.


### ReturnValueHandler
- HandlerMethodReturnValueHandler 를 줄여서 ReturnValueHandler 라 부른다.
- ArgumentResolver 와 비슷한데, 이것은 응답 값을 변환하고 처리한다.
- 컨트롤러에서 String으로 뷰 이름을 반환해도, 동작하는 이유가 바로 ReturnValueHandler 덕분이다.
- 어떤 종류들이 있는지 살짝 코드로 확인만 해보자.
스프링은 10여개가 넘는 ReturnValueHandler 를 지원한다.
예) ModelAndView , @ResponseBody , HttpEntity , String


![MVC구조](/image/메시지컨버터위치.png)

- 요청의 경우 @RequestBody 를 처리하는 ArgumentResolver 가 있고, HttpEntity 를 처리하는 ArgumentResolver 가 있다. 
- 이 ArgumentResolver 들이 HTTP 메시지 컨버터를 사용해서 필요한
객체를 생성하는 것이다. (어떤 종류가 있는지 코드로 살짝 확인해보자)

응답의 경우 @ResponseBody 와 HttpEntity 를 처리하는 ReturnValueHandler 가 있다. 그리고
여기에서 HTTP 메시지 컨버터를 호출해서 응답 결과를 만든다.
<br>
스프링 MVC는 @RequestBody @ResponseBody 가 있으면
RequestResponseBodyMethodProcessor (ArgumentResolver)
HttpEntity 가 있으면 HttpEntityMethodProcessor (ArgumentResolver)를 사용한다



