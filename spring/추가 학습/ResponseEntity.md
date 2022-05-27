# ResponseEntity



## 소개



### **RequestEntity** / ResponseEntity 란

- HttpEntity : HTTP 요청 / 응답에 해당하는 HttpHeader와 HttpBody를 포함하는 클래스

- ResponseEntity : HttpEntity를 상속받아 **HttpStatus, HttpHeaders, HttpBody**를 포함

  => 데이터 응답에 더 세밀한 제어 O



## VO



### VO

- DTO와 비슷한 개념이지만 read-only 속성 가짐
- 사용 용도
  - 사용자의 데이터 입력이 왔을 때 데이터나 조회 조건을 VO에 담아 DAO에 전달
  - HttpBody에 넣어서 전달한 데이터 생성
- 필요성
  - Network traffic 줄임에 효과적 (?)







### ModelMapper

```xml
<dependency>
    <groupId>org.modelmapper</groupId>
    <artifactId>modelmapper</artifactId>
    <version>2.3.8</version>
</dependency>
```

- 어떤 Object에 있는 필드값들을 자동으로 원하는 Object로 Mapping 시킴

- strategy
  - STRICT : 정확히 일치 필드만 매핑
  - STANDARD : 지능적 매핑
  - LOOSE : 느슨하게 매핑

```JAVA
ModelMapper mapper = new ModelMapper();
mapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STRICT);

UserDto userDto = mapper.map(user, UserDto.class);

ResponseUser responseUser = mapper.map(userDto, ResponseUser.class);
```





















