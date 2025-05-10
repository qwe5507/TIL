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
