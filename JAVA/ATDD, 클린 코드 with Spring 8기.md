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
     -  좋은 퀄리티의 테스트 결과 를 얻을수 있다.
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

---
## 4. 인수 테스트와 리팩터링
- [🚀 실습 - Cucumber 전환](https://github.com/next-step/atdd-subway-fare/pull/427)
- [🚀 1단계 - 경로 조회 타입 추가](https://github.com/next-step/atdd-subway-fare/pull/435)
- [🚀 2단계 - 요금 조회](https://github.com/next-step/atdd-subway-fare/pull/438)
- [🚀 3단계 - 요금 정책 추가](https://github.com/next-step/atdd-subway-fare/pull/461)
