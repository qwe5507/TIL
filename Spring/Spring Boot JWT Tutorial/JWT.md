> ###  [인프런 Spring Boot JWT Tutorial](> ###  [진짜 입문자를 위한 클라우드와 AWS](https://www.inflearn.com/course/aws-starter/dashboard)을 보고 정리

<br>

### **JWT**
- RFC 7519 웹 표준
- JSON 객체를 사용해서 토큰 자체에 정보들을 저장하고 있는 Web Token

### **JWT 구성요소**
- Header
  - Signature를 해싱하기 위한 알고리즘 정보가 담겨 있다.
- Payload
  - 시스템에서 실제로 사용될 데이터
- Signature
  - 토큰의 유효성 검증을 위한 문자열
  - 이 문자열을 통해 서버에서 토큰이 유효한 토큰인지 검증한다.
  
### **JWT 장점**
- 중앙의 인증서버, 데이터 스토어에 대한 의존성 없음
- 시스템 수평 확장에 유리
- Base64 URL Safe Encoding을 이용하기 때문에 URL, Cookie, Header등 어디에든 사용 할 수 있는 범용성이 있다.
> Base64 URL Safe Encoding이란 Base64 색인표의 62번(+), 63번(/)를 -(minus), _(underline)으로 변경 한 url safe한 Encoding방식 


### **JWT 단점**
- Payload의 정보가 많아지면 네트워크 사용량 증가하므로 고려해서 설계 해야 한다.
- 토큰이 클라이언트에 저장, 서버에서 클라이언트의 토큰을 조작할 수 없다.

<br>

---

### 프로젝트 생성
- Spring Boot 2.6.6

*의존성*
- Spring Web
- Spring Security
- Srping Data JPA
- H2 Database
- Lombok
- Validation

`Spring Security 의존성 추가 시 일반 api요청 시 401(Unauthorized)발생`
- 스프링 시큐리티를 추가한 순간 모든 요청에 대해 인증이 필요하게 된다.
- 401(Unauthorized)은 403(Forbidden, 권한 없음)과 비슷하지만, 401의 경우에는 인증이 가능합니다.
  - 403은 계속 인증 요청해도 계속 fail


