# hanghe99-spring-homework

## 목표

- Spring Boot 기반 CRUD REST API를 생성 가능
- 인증/인가를 이해하고, JWT를 활용해서 게시판을 처리
- JPA 연관관계를 이해하고 회원과 게시글을 구현

## 필수 요구사항

### API 명세서 작성과 CRUD 구현

- [Use Case 참고해서 작성](https://narup.tistory.com/70)
- `전체 Board 조회 API`
  - title, writer, content, created_at
  - 작성 날짜 기준 내림차순 정렬
- `Board 작성 API`
  - title, writer, password, content
  - 저장된 Board를 Client로 반환
- `단건 Board 조회 API`
  - title, writer, content, created_at
- `단건 Board 수정 API`
  - 수정 시 title, writer, content + password로 같이 보내서 서버에서 비밀번호 일치 여부 확인
  - 수정된 Board를 Client로 반환
- `단건 Board 삭제 API`
  - 삭제 요청 시, Password를 같이 보내서 서버에서 비밀번호 일치 여부를 확인 후 삭제
  - 삭제 시, Client로 성공했다는 표시 반환
- `Postman으로 QA 테스트 진행`
- `고민하기`
  - 수정, 삭제 API의 request를 어떤 방식으로 사용했는지(param, query, body)
  - 어떤 상황에 어떤 방식의 request를 사용해야 하는지
  - RESTful한 API를 설계 했는지? 어떤 부분이 해당되고 어떤 부분이 해당되지 않는지
  - 적절한 관심사 분리를 적용했는지(Controller, Service, Repository)
  - API 명세서 작성 가이드라인을 검색하여 직접 작성한 API와 비교하기

### AWS 배포, 회원가입과 로그인 구현

- `AWS 배포`
  - EC2 배포
  - 배포 후 IP주소를 제출
  - 도메인 주소를 연결한다면 GOOD

- `회원가입 API`
  - username, password를 Client에서 전달받기
  - username 구성
    - 최소 4자 이상, 10자 이하
    - 알파벳 소문자
    - 숫자
  - password 구성
    - 최소 8자 이상, 15자 이하
    - 알파벳 대소문자
    - 숫자
  - DB에 중복된 username이 없으면 Client를 저장 후, 성공 메시지 + 201 반환
    - [참고문서 1](https://mangkyu.tistory.com/174)
    - [참고문서 2](https://ko.wikipedia.org/wiki/%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D)
    - [참고문서 3](https://bamdule.tistory.com/35)
- `로그인 API`
  - username, password를 Client에서 전달받기
  - DB에서 username을 사용하여 저장된 회원의 유무를 확인하고 있다면 password 비교하기
  - 로그인 성공 시, 로그인에 성공한 유저의 정보와 JWT를 활용하여 토큰을 발급하고, 발급한 토큰을 Header에 추가하고 성공했다는 메시지, HttpStatus Code와 함께 Client 반환
- `Postman으로 QA 테스트 진행`
- `API 명세서와 ERD 설계 방법`
  - [ERD](https://www.erdcloud.com/)
  - [API 명세서](https://learnote-dev.com/java/Spring-A-%EB%AC%B8%EC%84%9C-%EC%9E%91%EC%84%B1%ED%95%98%EA%B8%B0/)
- `고민하기`
  - 처음 설계한 API 명세서에 변경이 있었나
  - ERD를 먼저 설계한 후 Entity를 개발할 때 어떤 점이 도움이 되었는지
  - JWT를 사용하여 인증/인가를 구현 했을 때의 장점은 무엇인지
  - 반대로 JWT를 사용한 인증/인가의 한계점은 무엇인지
  - 만약 댓글 기능이 있는 블로그에서 댓글이 달려있는 Board를 삭제하려고 한다면 무슨 문제가 발생할지, DB 테이블 관점에서 해결방법이 있는지
  - IoC / DI에 대해 알고 있는지

## 추가 요구사항

### 추가 API

- `회원가입 API`
  - username, password를 Client에서 전달받기
  - username 구성
    - 최소 4자 이상, 10자 이하
    - 알파벳 소문자
    - 숫자
  - password 구성
    - 최소 8자 이상, 15자 이하
    - 알파벳 대소문자
    - 숫자
    - 특수문자
  - DB에 중복된 username이 없다면 회원을 저장, Client로 성공 메시지와 상태코드 반환
    - [참고문서 1](https://mangkyu.tistory.com/174)
    - [참고문서 2](https://ko.wikipedia.org/wiki/%EC%A0%95%EA%B7%9C_%ED%91%9C%ED%98%84%EC%8B%9D)
    - [참고문서 3](https://bamdule.tistory.com/35)
  - 회원 권한 부여 
    - ADMIN: 모든 게시글 수정 및 삭제 가능 
    - USER
- `로그인 API`
  - username, password를 Client로 전달받기
  - DB에서 username을 사용하여 저장된 회원의 유무를 확인하고 있다면 password로 비교
  - 로그인 성공 시, 로그인에 성공한 유저의 정보와 JWT를 활용하여 토큰을 발급, 발급한 토큰을 Header에 추가하고 성공했다는 메시지와 상태코드를 Client에게 반환
- `댓글 작성 API`
  - 토큰을 검사하여, 유효한 토큰일 경우에만 댓글 작성 가능
  - 선택한 게시글의 DB 저장 유무를 확인
  - 선택한 게시글이 있다면 댓글을 등록하고 등록된 댓글 반환
- `댓글 수정 API`
  - 토큰을 검사한 후, 유효한 토큰이면서 해당 사용자가 작성한 댓글만 수정 가능
  - 선택한 댓글의 DB 저장 유무를 확인하기
  - 선택한 댓글이 있다면 댓글 수정하고 수정된 댓글 반환하기
- `댓글 삭제 API`
  - 토큰을 검사한 후, 유효한 토큰이면서 해당 사용자가 작성한 댓글만 삭제 가능
  - 선택한 댓글의 DB 저장 유무를 확인하기
  - 선택한 댓글이 있다면 댓글 삭제하고 Client 로 성공했다는 메시지, 상태코드 반환하기
- `예외 처리`
  - 토큰이 필요한 API 요청에서 토큰을 전달하지 않았거나 정상 토큰이 아닐 때는 "토큰이 유효하지 않습니다." 라는 에러메시지와 statusCode: 400을 Client에 반환하기
  - 토큰이 있고, 유효한 토큰이지만 해당 사용자가 작성한 게시글/댓글이 아닌 경우에는 “작성자만 삭제/수정할 수 있습니다.”라는 에러메시지와 statusCode: 400을 Client에 반환하기
  - DB에 이미 존재하는 username으로 회원가입을 요청한 경우 "중복된 username 입니다." 라는 에러메시지와 statusCode: 400을 Client에 반환하기
  - 로그인 시, 전달된 username과 password 중 맞지 않는 정보가 있다면 "회원을 찾을 수 없습니다."라는 에러메시지와 statusCode: 400을 Client에 반환하기

### 추가 수정

- `전체 게시글 목록 조회 API`
  - title, username, content, created_at를 조회하기
  - 작성 날짜 기준 내림차순으로 정렬하기
  - 각각의 게시글에 등록된 모든 댓글을 게시글과 같이 Client에 반환하기
  - 댓글은 작성 날짜 기준 내림차순으로 정렬하기
- `게시글 작성 API`
  - 토큰을 검사하여, 유효한 토큰일 경우에만 게시글 작성 가능
  - 제목, 작성자명(username), 작성 내용을 저장하고
  - 저장된 게시글을 Client 로 반환하기
- `선택한 게시글 조회 API`
  - 선택한 게시글의 제목, 작성자명(username), 작성 날짜, 작성 내용을 조회하기 
(검색 기능이 아닙니다. 간단한 게시글 조회만 구현해주세요.)
  - 선택한 게시글에 등록된 모든 댓글을 선택한 게시글과 같이 Client에 반환하기
  - 댓글은 작성 날짜 기준 내림차순으로 정렬하기
- `선택한 게시글 수정 API`
  - 수정을 요청할 때 수정할 데이터와 비밀번호를 같이 보내서 서버에서 비밀번호 일치 여부를 확인 한 후
  - 토큰을 검사한 후, 유효한 토큰이면서 해당 사용자가 작성한 게시글만 수정 가능
  - 제목, 작성 내용을 수정하고 수정된 게시글을 Client 로 반환하기
- `선택한 게시글 삭제 API`
  - 삭제를 요청할 때 비밀번호를 같이 보내서 서버에서 비밀번호 일치 여부를 확인 한 후
  - 토큰을 검사한 후, 유효한 토큰이면서 해당 사용자가 작성한 게시글만 삭제 가능
  - 선택한 게시글을 삭제하고 Client 로 성공했다는 메시지, 상태코드 반환하기

## Warning

### Do

- 필수 요구 사항

### Don't

- 배포 시 AWS의 RDS를 사용했다면 GitHub에 절대 RDS에 대한 정보를 올리지 말 것
- 프론트 엔드 페이지 설계 X, API에 집중

## 참고자료

- `입문`
  - [웹 동작방식 이해](https://www.notion.so/teamsparta/1512dc3ef514814ebb6dcb7774d8d9b0?pvs=25)
  - [SpringBoot 및 서버와 이해](https://www.notion.so/teamsparta/SpringBoot-1512dc3ef51481dead63f11d5cb198a7?pvs=25)
  - [Database와 SQL](https://www.notion.so/teamsparta/Database-SQL-1512dc3ef51481ac94f9e3198e9f2d60?pvs=25)
  - [Project Memo Part 1](https://www.notion.so/teamsparta/Project-Memo-Part-1-1512dc3ef51481daad17d94e79ef7df4?pvs=25)
  - [Ioc, DI](https://www.notion.so/teamsparta/IoC-DI-1512dc3ef51481e1937be976f6852f33?pvs=25)
  - [Project Memo Part 2](https://www.notion.so/teamsparta/Project-Memo-Part-2-1512dc3ef514817bb6f5d866b31f334d?pvs=25)
  - [Bonus 자료](https://www.notion.so/teamsparta/Bonus-1512dc3ef514819382e4ca6e1c8a2daf?pvs=25)
- `숙련`
  - [JPA 심화](https://www.notion.so/teamsparta/JPA-1512dc3ef51481608158de060d6a0cb0?pvs=25)
  - [Project MySelectShop - Prepare](https://www.notion.so/teamsparta/Project-MySelectShop-Prepare-1512dc3ef51481d29c86c559d75503c2?pvs=25)
  - [Project MySelectShop - AllInOne](https://www.notion.so/teamsparta/Project-MySelectShop-AllInOne-1512dc3ef51481859c90fc903a87855a?pvs=25)
  - [Project MySelectShop - Refactoring](https://www.notion.so/teamsparta/Project-MySelectShop-Refactoring-1512dc3ef5148143977ce7c10f3ce1f5?pvs=25)
  - [Project MySelectShop - Auth](https://www.notion.so/teamsparta/Project-MySelectShop-Auth-1512dc3ef5148131b9e8cb7e3c6e7ecf?pvs=25)
  - [Project MySelectShop - JWT](https://www.notion.so/teamsparta/Project-MySelectShop-JWT-1512dc3ef51481c39b60d3852d77d744?pvs=25)
  - [Project MySelectShop - JPA](https://www.notion.so/teamsparta/Project-MySelectShop-JPA-1512dc3ef51481e29652f7497154123e?pvs=25)
  - [Bonus 자료](https://www.notion.so/teamsparta/Bonus-1512dc3ef51481eda183f48b0bda3a70?pvs=25)
- `심화`
  - [Spring Security](https://www.notion.so/teamsparta/Spring-Security-1512dc3ef5148174af0ced45f1422d45?pvs=25)
  - [Project MySelectShop - Security](https://www.notion.so/teamsparta/Project-MySelectShop-Security-1512dc3ef51481f9afc0f90ada541035?pvs=25)
  - [Project MySelectShop - OAuth2](https://www.notion.so/teamsparta/Project-MySelectShop-OAuth2-1512dc3ef514817287afc516a74fbcb6?pvs=25)
  - [Spring Test](https://www.notion.so/teamsparta/Spring-Test-1512dc3ef5148121a91acb24e283f619?pvs=25)
  - [Spring AOP](https://www.notion.so/teamsparta/Spring-AOP-1512dc3ef514815da62ce78d7e67fd42?pvs=25)
  - [Spring Exception](https://www.notion.so/teamsparta/Spring-Exception-1512dc3ef51481b29ddbd3328456a0a0?pvs=25)
  - [Spring Transaction](https://www.notion.so/teamsparta/Spring-Transaction-1512dc3ef51481a09016f72e01d89bc1?pvs=25)