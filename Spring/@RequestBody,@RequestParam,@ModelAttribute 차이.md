##RequestBody
- RequestBody는 클라이언트가 전송하는 JSON(application/json) 형태의 HTTP Body 내용을 Java Object로 변환시켜주는 역할
- @RequestBody로 받은 데이터는 Spring에서 관리하는 MessageConverter들 중 하나인 MappingJackson2 HttpMessageConverter를 통해 Java 객체로 변환된다.
- Spring은 메시지를 변환하는 과정에서 객체의 기본 생성자를 통해 객체를 생성하고 내부적으로 Reflection을 사용해 값을 할당하므로 @RequestBody에는 값을 주입하기 위한 생성자나 Setter가 필요 없다.


##RequestParam
- HTTP 요청의 Body를 직접 조회하지 않는다.
- @RequestParam은 1개의 HTTP 요청 파라미터를 받기 위해서 사용된다.

##ModelAttribute
- @ModelAttribute를 사용하여 HTTP Body에 내용을 담기 위해서는 multipart/form-data 형식으로 전송해야 한다.