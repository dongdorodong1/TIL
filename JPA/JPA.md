# JPA
![](/image/JPA_%EC%98%81%EC%86%8D%EC%84%B1%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8.png)

> em.persist : 영속성 컨텍스트에 저장,
> 쓰기지연SQL저장소에 SQL 저장하기
> ![](/image/%EC%93%B0%EA%B8%B0%EC%A7%80%EC%97%B0SQL%EC%A0%80%EC%9E%A5%EC%86%8C.png) 


## 양방향 매핑
- 연관관계의 주인은 외래키를 가지고 있는 엔티티로 선정하기
- 관계추가시 편의메서드를 사용하여 하나의 방향에서 연관관계까지 추가할수 있도록 설계
- 컨트롤러에서는 Entity를 반환하지마라, DTO를 따로 만들어서 하는게 좋음

- 다대일 관계를 제일 많이 사용

일대다 관계는 권장하지 않음
- 왜? 운영이 힘들어짐 연관관계주인이 바뀌기 때문에 코드분석이 어려워진다. 

![](/image/%EC%93%B0%EA%B8%B0%EC%A7%80%EC%97%B0SQL%EC%A0%80%EC%9E%A5%EC%86%8C.png) 


![](/image/%EB%8B%A4%EB%8C%80%EB%8B%A4%20%ED%95%B4%EA%B2%B0.png) 

![](/image/%EC%83%81%EC%86%8D%EA%B4%80%EA%B3%84Mapping.png)
![](/image/%EC%83%81%EC%86%8D%EA%B4%80%EA%B3%842.png)

- @DiscriminatorColumn(name="DTYPE"), DTYPE이 기본값.
- 상위클래스에 어떤 하위객체가 저장되었는지 나타내는 컬럼 
  