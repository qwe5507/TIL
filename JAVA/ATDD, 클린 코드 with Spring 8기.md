# ATDD, 클린 코드 with Spring 8기
`ATDD, 클린 코드 with Spring 8기`를 진행하며 피드백을 정리하고 해당 과정을 통해 배운점을 정리

--- 

## 1. 인수 테스트와 E2E 테스트
- [🚀 2단계 - 지하철 노선 관리](https://github.com/next-step/atdd-subway-map/pull/1057)
- [🚀 3단계 - 지하철 구간 관리](https://github.com/next-step/atdd-subway-map/pull/1091)

**1. 인수테스트는 시나리오가 잘 드러나야 한다.**

**Before**
```java
     /**
     * When 지하철 노선을 생성하면
     * Then 지하철 노선 목록 조회 시 생성한 노선을 찾을 수 있다
     */
    @DisplayName("노선을 생성한다.")
    @Test
    void apiCreateLine() {
        // when
        Map<String, String> params = createLineRequestPixture("신분당선", "bg-red-600", 1L, 2L);

        ExtractableResponse<Response> response = RestAssured.given().log().all()
                .body(params)
                .contentType(MediaType.APPLICATION_JSON_VALUE)
                .when().post("/lines")
                .then().log().all()
                .extract();

        // then
        assertThat(response.statusCode()).isEqualTo(HttpStatus.CREATED.value());

        // then
        final ExtractableResponse<Response> extract = apiGetLines();
        assertThat(extract.jsonPath().getList("name")).contains("신분당선");
        assertThat(extract.jsonPath().getList("color")).contains("bg-red-600");
    }
```

**After**
```java
     /**
     * When 지하철 노선을 생성하면
     * Then 지하철 노선 목록 조회 시 생성한 노선을 찾을 수 있다
     */
    @DisplayName("노선을 생성한다.")
    @Test
    void apiCreateLine() {
        // when
        노선이_생성되어_있다("신분당선", "bg-red-600", 1L, 2L);

        // then
        final ExtractableResponse<Response> extract = 노선목록을_조회한다();
        assertThat(extract.jsonPath().getList("name")).contains("신분당선");
        assertThat(extract.jsonPath().getList("color")).contains("bg-red-600");
    }

    public static ExtractableResponse<Response> 노선목록을_조회한다() {
        return RestAssured.given().log().all()
                .contentType(MediaType.APPLICATION_JSON_VALUE)
                .when().get("/lines")
                .then().log().all()
                .statusCode(HttpStatus.OK.value())
                .extract();
    }

    public static ExtractableResponse<Response> 노선을_조회한다(final Long lineId) {
        return RestAssured.given().log().all()
                .contentType(MediaType.APPLICATION_JSON_VALUE)
                .when().get("/lines/" + lineId)
                .then().log().all()
                .statusCode(HttpStatus.OK.value())
                .extract();
    }
```
- 시나리오가 인수테스트에 잘 드러날수 있게 한글메소드를 활용하여 작성한다.

--- 
## 2. 인수 테스트와 TDD
- [🚀 실습 - 단위 테스트 작성](https://github.com/next-step/atdd-subway-path/pull/694)
- [🚀 1단계 - 구간 추가 요구사항 반영](https://github.com/next-step/atdd-subway-path/pull/707)
- [🚀 2단계 - 구간 제거 요구사항 반영](https://github.com/next-step/atdd-subway-path/pull/716)
- [🚀 3단계 - 경로 조회 기능](https://github.com/next-step/atdd-subway-path/pull/728)

**1. 상황에 따른 테스트 방식 고려 (Classic TDD vs Mockist TDD)**
- **Classic TDD(Inside Out)**
     -  실제 객체를 조합하며 작은 단위부터 테스트를 시작해 전체를 완성하는 방식
     -  도메인에 대한 이해가 부족할때 실행하기 어려울 수 있다.
     -  의존성이 많을 경우 테스트가 복잡해지고 느릴 수 있습니다.
     -  좋은 퀄리티의 테스트 결과를 얻을수 있다.
- **Mockist TDD(Outside in)**
     - 외부에서 테스트를 작성하고 하위 컴포넌트를 Mock으로 설계하며 내려가는 방식
     - 협력객체가 어떻게 동작할지 생각하면서 진행
     - 빠르고 인터페이스 중심의 유연한 설계가 가능하지만 과도한 Mock 사용은 테스트 유지보수를 어렵게 만들 수 있습니다.
     - 요구사항의 이해도를 높인 테스트를 얻을 수 있다. 
- 둘 중 하나를 선택해야하는 문제가 아니다.
     - 컨트롤러를 먼저 만들것인지, 도메인을 먼저 만들지 정답은 없음
     - 도메인에 대한 이해가 되어 객체설계가 가능하다면 처음부터 Classic TDD방식으로 진행해도 된다.
     - 그러나 객체설계가 어렵다면 Mockist TDD방식으로 외부부터 작성하다가 막히는 부분이 생기면 그때부터 Classic TDD로 도메인을 작성해도 된다.
     - 본인에게 혹은 상황에맞는 테스트 방식을 고르는게 중요하다.

**2. 예외 메시지도 검증하라.**
```java
    @DisplayName("노선의 중간에 구간을 추가할 때 기존구간의 길이보다 추가되는 길이가 같거나 크면 오류가 발생한다.")
    @Test
    void addSection_middle_invalid_distance() {
        assertThatExceptionOfType(ResponseStatusException.class)
                .isThrownBy(() -> 이호선.addSection(역삼역, 선릉역, 10));
     }
```
```java
    @DisplayName("노선의 중간에 구간을 추가할 때 기존구간의 길이보다 추가되는 길이가 같거나 크면 오류가 발생한다.")
    @Test
    void addSection_middle_invalid_distance() {
        assertThatThrownBy(() -> { 이호선.addSection(역삼역, 선릉역, 10); })
                .isInstanceOf(ResponseStatusException.class)
                .hasMessageContaining("중간에 추가되는 길이가 상행역의 길이보다 크거나 같을 수 없습니다.");
    }
```
**3. 테스트코드도 유지보수 대상이다.**
- 테스트는 코드 변경 시 함께 수정되어야 하므로, 유지보수 비용을 고려해 필요한 부분만 테스트해야 한다.
- 단순 위임만 하거나 단순한 값 객체는 별도 테스트 없이 상위 로직에서 간접 테스트되는 것으로 충분할 수 있다.
  
---
## 3. 인수 테스트와 인증
- [🚀 실습 - 인증 기반 인수 테스트](https://github.com/next-step/atdd-subway-favorite/pull/561)
- [🚀 1단계 - 즐겨찾기 기능 완성](https://github.com/next-step/atdd-subway-favorite/pull/569)
- [🚀 2단계 - 깃헙 로그인 구현](https://github.com/next-step/atdd-subway-favorite/pull/573)
- [🚀 3단계 - 패키지 리팩터링](https://github.com/next-step/atdd-subway-favorite/pull/583)

**1. 의존은 무조건 나쁘지 않다.**
- 의존성이란 B가 변경될 때 A가 변경 되는 것
- 의존이 없으면 하나의 객체에서 모든 것을 처리해야 한다.
- 적절한 범위로 책임을 나누어 의존하게 해야한다.

**2. 의존성에 대한 테스트는 통합테스트로 테스트하라.**
- 단위 테스트로 시스템 전반적인 것을 테스트 하기 어렵다.
- e2e테스트를 통해 단위 테스트로 검증 할 수 없는 것을 테스트 할 수 있지만 전부 e2e로 검증하려하면 비용이 과할 수 있다.
- 통합테스트란 독립된 단위가 서로 연결되었을 때, 연결된 부분이 잘 동작하는지 검증하는 테스트를 보편적으로 통합 테스트라고 한다.
     -  `통합테스트를 얘기할 때, 인풋부터 시작해서 모든영역을 통합해서 검증하는걸 통합테스트가 아니었나?`
          -  통합테스트도 크게 두가지로 부를 수 있다.
               - 서비스의 모든 부분을 거치는 범위를 가지는 (넓은 의미)
                   - 여러 모듈로 구성되는 시스템이 예상대로 동작하는지 검증
               - 외부와 접하고 있는 내 영역만 가지고 외부와 함께 잘 동작하는지 검증하는 (좁은 의미)
                   - 외부 의존성이 있는 별도로 개발된 모듈이 잘 동작하는지 검증
          - 단위와 통합이라는 용어는 옛날의 규모가 참작았을때부터 쓰다가 현재는 규모가 커져서 개념적으로 혼잡한 부분이 있다. 
               - 용어에 매몰되지 말라.
               - 통합 테스트라고 해서 꼭 전체를 테스트할 필요는 없다.
               - 외부와 접하고 있는 부분만 테스트를 해도 통합테스트라고 할 수 있다.
- 의존성 테스트 방법
     - 실제 외부 의존성 사용
     - Stub으로 대체
     - Fake방식으로 대체

**3. 인수 조건을 상세히 나누어라.**

**Before**

```
    /**
     * When code로 깃헙을 통한 로그인 API를 요청한다.
     * Then 200 코드를 리턴한다.
     * And accessToken을 리턴한다.
     */
```

**After**

`회원가입 한 사용자가 로그인 요청하는 경우`
```
     WHEN 회원가입한 사용자가 로그인 요청하는 경우
     THEN 액세스토큰을 응답한다
```
`회원가입하지 않은 사용자가 로그인 요청하는 경우`
```
     WHEN 회원가입하지 않은 사용자가 로그인 요청하는 경우
     THEN 회원가입이 완료된다 (멤버 조회가 정상적으로 되는지를 검증해본다)
     THEN 액세스토큰을 응답한다
```



---
## 4. 인수 테스트와 리팩터링
- [🚀 실습 - Cucumber 전환](https://github.com/next-step/atdd-subway-fare/pull/427)
- [🚀 1단계 - 경로 조회 타입 추가](https://github.com/next-step/atdd-subway-fare/pull/435)
- [🚀 2단계 - 요금 조회](https://github.com/next-step/atdd-subway-fare/pull/438)
- [🚀 3단계 - 요금 정책 추가](https://github.com/next-step/atdd-subway-fare/pull/461)

**1. 테스트하기 쉬운 코드로 개발하라.**
- 외부 의존성은 분리하라
     - 비즈니스 로직과 외부 의존을 명확히 구분해야 테스트가 쉬워진다.
     - 핵심 도메인 로직만 남긴 코드는 테스트하기 쉬워진다
     - 외부 의존이 없는 순수한 로직만 테스트 대상으로 남기면 단위 테스트가 가능하다.
     - 그렇다고 외부 의존을 완전히 제거할 순 없고, 그런 부분은 인수 테스트로 검증한다.
- MockBean/SpyBean을 남용하기보다 코드 구조 개선 우선
- 테스트 코드가 복잡해지고 유지가 어려워진다면, 테스트가 아니라 프로덕션 코드를 개선할 시점
- 애초에 테스트하기 쉽게 코드를 짜는 것이 핵심이다.
- 완벽한 테스트보다, 가능한 테스트를 우선
     - 완벽하게 분리되지 않더라도 프로덕션 코드에 약간 의존해도 테스트하는 게 낫다.
     - `상태 검증(stub)` → `행위 검증(mock)` → `fake` → `실제 객체` 순으로 점진적으로 테스트 전략을 조정
- “테스트하기 쉬운 코드”는 곧 “좋은 설계”
     - 외부 의존성을 어떻게 통제하고 분리할지 고민하는 것은 클린 코드와도 직결된다.
     - 테스트하기 어려운 코드는 베드 스멜일 가능성이 높다.

**2. 인수테스트 보호아래 리팩토링하라.**
- 인수 테스트를 기반으로 기능을 변경하면 기능이 깨졌는지 즉시 확인 가능함.
     - 하지만 테스트 코드가 많아지면 2번 일하는 느낌을 줄 수 있음 (개발 + 테스트 유지)
- 변화 유형에 따라 테스트 전략이 다르다
     - ```
          요구사항의 변경점이 있는 리팩토링:
          인수 테스트 추가(실패) -> 단위 테스트 추가(실패) -> 단위 기능 구현 -> 단위 테스트 성공 -> 추가한 인수 테스트 성공 -> 기존 인수테스트 및 단위 테스트 삭제

          요구사항의 변경점이 없는 리팩토링:
          인수 테스트 변경 -> 단위 테스트 변경 -> 단위 기능 구현 -> 단위 테스트 성공 -> 인수 테스트 성공
       ```
- 테스트 수정의 우선순위
     - 기존 테스트를 지우기보단 유지하거나 보완하는 것이 좋음
     - 특히 인수 테스트가 없다면, 기능 변경 전에 먼저 작성하는 게 권장됨
- 인수 테스트 없는 기능 변경 시 주의사항
     - 테스트 없이 기능을 바꾸는 건 위험 → 최소한의 인수 테스트라도 선 작성
     - 테스트 작성은 개발 프로세스의 일부로 봐야 함
- 실수하기 쉬운 시점
     - 리팩터링인 줄 알고 테스트 무시했는데 실제로는 스펙 변경이었을 수도 있음
     → 항상 먼저 "요구사항이 바뀌었는가?"를 스스로 체크해야 함

**3. 인수테스트 관리가 어렵다면 Cucumber라이브러리 사용**
- .feature 파일에 시나리오 작성 (Given-When-Then)
- Step 정의 클래스에서 각 시나리오 단계 구현
- 테스트 실행 시 feature와 Step 매핑되어 테스트 수행
- GLUE는 step 위치, PLUGIN은 출력 형식 설정
- HTTP 로그는 logback-access로 확인 가능
- 공통 Step은 분리해 재사용하면 좋음

