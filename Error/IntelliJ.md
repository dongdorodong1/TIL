
## 23.03.18
- IntelliJ로 Spring MVC 프로젝트 빌드 중 계속해서 My Batis 에러가 났다. 
  에러내용은 아래와 같다.
<br>
   Error updating database.  Cause: java.lang.IllegalArgumentException: Mapped Statements collection does not contain value for com.test.CountValue

아무리 구글링을 해봐도 문제를 해결할 수 없었음. 

- 확인해본것. 
> 1. mapper의 경로
> 2. xml에 작성된 namespace, id와 작성한 메소드와의 비교 
> 3. 스펠링
> 4. namespace 내부의 중복 id

인터넷에 나와있는 건 다해본 것같다. 

###해결방법
이클립스에서 같은 프로젝트를 띄웠는데 문제 없이 돌아감 

원인은 IntelliJ에 있었음. 
인텔리제이는 이클립스와 달리 xml파일을 기본적으로 포함시켜주지 아않는다고 한다. 

pom.xml 파일 \<build> 안쪽에
```
<resources>
       <resource>
	    	<directory> src/main/java </directory>
			<includes>
				<include>**/*.xml</include>
			</includes>
</resource>
```
를 넣어주니 해결되었다.
  