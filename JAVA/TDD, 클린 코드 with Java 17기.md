# TDD, 클린 코드 with Java 17기
`ATDD, 클린 코드 with Spring 8기`를 진행하기 전 앞서 진행한 `TDD, 클린 코드 with Java 17기` 의 피드백을 정리하고 해당 과정을 통해 배운점을 정리

---

## 1. 자동차 경주 - 단위테스트 피드백 
- [🚀 2단계 - 문자열 덧셈 계산기](https://github.com/next-step/java-racingcar/pull/4835)
- [🚀 3단계 - 자동차 경주](https://github.com/next-step/java-racingcar/pull/4933)
- [🚀 4단계 - 자동차 경주(우승자)](https://github.com/next-step/java-racingcar/pull/4984)
- [🚀 5단계 - 자동차 경주(리팩토링)](https://github.com/next-step/java-racingcar/pull/5060)

**1. 직관적인 메소드 네임 사용**
```java
    public static void println() { // X
```
```java
    public static void emptyLine() {
```
- `빈 줄`을 출력하는 메소드의 네이밍을 `println()`보다는 `emptyLine()`으로 좀 더 직관적인 이름을 선택해라.

**2. 무작위한 값을 검증 할때는 `@RepeatedTest` 를 사용**

**3. 역할 분리**
- 처음에는 Car라는 클래스의 위치 변수를 `-` 로 가지고 있다가 출력하였으나 출력을 담당하는 클래스와 분리하여 Car의 위치 상태는 정수형으로 표현 후 출력의 역할을 담당하는 클래스가 출력할 때 `-`로 변환하여 출력하게 변경

```java
public class Car {
    private final static String STEP_STRING = "-"; // X
    
```
- 위와 같은 코드는 view를 Car 내부에 표현했다고 볼 수 있다.

**4. 테스트가 어렵다면 역할이 과한지 의심해보자.**
```java
private void goOrStop() {
    final int randomInt = new Random().nextInt(10);
    if (randomInt > 4) {
        this.step.append(STEP_STRING);
    }
}
```
- 처음엔 핵심 도메인의 비즈니스 로직을 위와 같은 메소드로 작성하였다, 그러나 메소드안 new Randome.nextInt(10)은 테스트를 어렵게 하는 로직이다.
- 테스트를 쉽게 하기 위해서는, Random 값을 통제할 수 있는 상황으로 만드는 것이 좋다.
- Random한 값을 외부에서 받아서 처리하면 Random한 값을 통제할 수 있어 테스트가 용이해지고, 외부의 Random값을 생성하는 메소드는 0~10 사이의 랜덤 값이 추출되는지만 테스트 한다.

```java
private void goOrStop(MoveStrategy moveStrategy) {
    if (moveStrategy.getNumber() > 4) {
        this.step.append(STEP_STRING);
    }
}
```
- 외부의 `MoveStrategy`를 구현한 `RandomMoveStrategy`가 랜덤한 값을 주입해주므로 테스트가 용이해진다.

**5. 컬렉션 이름은 변수명이나 클래스명으로 사용하지 않는 것이 좋다.**
```java
public class CarList { // X
```

```java
private List<Double> numbers = new ArrayList<>();
private List<String> operators = new ArrayList<>();
```
- List, Collection 등의 자료형은 복수형으로 표현하는 것이 좋다.
- [https://tecoble.techcourse.co.kr/post/2020-04-24-variable_naming/](https://tecoble.techcourse.co.kr/post/2020-04-24-variable_naming/)

**6. getter는 가급적 사용하지 않는 것이 좋다.**
- 객체지향적으로 코드를 짜고, 메시지를 보내 상호작용을 한다고 하더라도 결국 VO는 getter를 해야 만 하는 순간이 온다.
- 무조건 사용하지 않는 것이 아닌, 가급적 사용을 최소화 하자.
- setter는 물론 사용하지 않는 것이 좋다.(불변)

---

## 2. 로또 - TDD 피드백
- [🚀 1단계 - 문자열 계산기](https://github.com/next-step/java-lotto/pull/3326)
- [🚀 2단계 - 로또(자동)](https://github.com/next-step/java-lotto/pull/3338)
- [🚀 3단계 - 로또(2등)](https://github.com/next-step/java-lotto/pull/3373)
- [🚀 4단계 - 로또(수동)](https://github.com/next-step/java-lotto/pull/3410)

**1. 숫자를 표기할때 언더스코어(_)를 포함해서 표기하면 가독성이 좋아진다.**
```java
    SIX_PRICE(6, new BigDecimal("2000000000")), // X
    SIX_PRICE(6, new BigDecimal("2_000_000_000")),
```
- 자바7 부터는 숫자 리터럴에 언더스코어(또는 언더바)를 사용하여 가독성을 높일 수 있다.

**2. static method만을 가지는 utility class는 private 생성자를 가지도록 구현한다.**
- public 기본 생성자를 제공하지 않고 다음과 같이 private 생성자로 구현하는 것이 잘못된 사용을 방지할 수 있다.
- [https://www.slipp.net/questions/360](https://www.slipp.net/questions/360)

**3. 테스트 값은 경계 값을 사용**
```java
@DisplayName("로또번호는 1이상 45이하 여야 한다.")
@ParameterizedTest
@ValueSource(ints = {-1, 46})
```
- 비즈니스 로직을 단위테스트 할 때 발생하는 오류는 경계값에서 주로 발생한다.
- 다른 결과가 도출될 수 있는 경계값으로 테스트 Picture를 만들어 진행하자.

---

## 3. 사다리타기 - FP, OOP 피드백

- [🚀 1단계 - 스트림, 람다, Optional](https://github.com/next-step/java-ladder/pull/1889)
- [🚀 2단계 - 사다리(생성)](https://github.com/next-step/java-ladder/pull/1912)
- [🚀 3단계 - 사다리(게임 실행)](https://github.com/next-step/java-ladder/pull/1938)
- [🚀 4단계 - 사다리(리팩터링)](https://github.com/next-step/java-ladder/pull/1977)

**1. 스트림은 직관적으로 사용하라**
```java
public static long sumOverThreeAndDouble(List<Integer> numbers) {
        return numbers.stream()
                .filter(n -> n > 3)
                .map(x -> x * 2)
                .reduce(0 , Integer::sum);
```
- 처음 작성 코드
```java
public static long sumOverThreeAndDouble(List<Integer> numbers) {
        return numbers.stream()
                .filter(StreamStudy::isOverThree)
                .map(StreamStudy:double)
                .reduce(0 , Integer::sum);
```
- 스트림을 사용 할 때는 어떤 동작을하는지 명시적으로 나타내어 주는게 좋다.
- Stream의 최대장점이 선언형으로 표현할 수 있다는 것

**2. 원시값 포장**
- 원시 값을 포장함으로써 얻는 장점
  - 객체지향 사고
  - 객체에 메시지를 보내 구현가능
  - 객체의 항상 올바른 값을 유지
  - 테스트하기 쉽다.
- 단점
  - 인스턴스가 많이 만들어진다.

**3. Bottop Up 방식 vs Top Down 방식**
- Bottom Up은 구현 후, 리팩토링으로 설계를 개선해 나가는 방식
  - TDD에 적합
  - Bottom Up 방식이더라도 완전히 설계 없이 구현에 들어가지 않는다, 약간의 설계 후 구현 후 리팩토링
- Top Down은 설계를 정교하게 한 후, 구현
  - 책임 주도 설계

어떤 방식이든 정답은 없으나 책임주도 설계가 어렵다면, TDD사이클을 반복해 설계의 품질을 높여나가면서 도메인지식을 쌓으면 인터페이스로 도출할 부분이 쉽게 보인다.

---

## 4.수강신청 - 레거시 코드 리팩터링 피드백
- [🚀 1단계 - 레거시 코드 리팩터링](https://github.com/next-step/java-lms/pull/232)
- [🚀 2단계 - 수강신청(도메인 모델)](https://github.com/next-step/java-lms/pull/303)
- [🚀 3단계 - 수강신청(DB 적용)](https://github.com/next-step/java-lms/pull/339)
- [🚀 4단계 - 수강신청(요구사항 변경)](https://github.com/next-step/java-lms/pull/356)

**1. Enum `values()` 성능 개선**
```java
public static ImageType of(final String name) {
        return Arrays.stream(values())
                .filter(n -> n.name.equals(name))
                .findFirst()
                .orElseThrow(() -> new IllegalArgumentException(INVALID_FILE_TYPE_MSG));
}
```
처음에는 위와같이 Enum의 정적 메소드에 `values()`를 사용하였다.
- 메소드가 호출 될때마다 `values()` 실행
  - `O(n)` 시간복잡도
    
고정된 값인 `values()`를 피드백을 받고 개선
```java
    private static final ImageType[] imageTypes = values();
    //...

    public static ImageType of(final String name) {
        return Arrays.stream(imageTypes)
                .filter(n -> n.name.equals(name))
                .findFirst()
                .orElseThrow(() -> new IllegalArgumentException(INVALID_FILE_TYPE_MSG));
```
Enum의 `values()`를 위와같이 변경
- 해당 step은 통과되었으나, 다시 생각해보니 filter에서 동일하게 O(n)이 발생하는건 같다고 생각되어 Map을 통해 개선

```java
private static final Map<String, ImageType> imageTypeMap =
        Collections.unmodifiableMap(Stream.of(values())
            .collect(Collectors.toMap(ImageType::getName(), Function.identity())));
```
- 위와 같이 Enum의 `name필드를 Key`로, `Enum을 value`인 Map을 생성하여 조회 시점에 Map에서 찾게 되면 조회 성능은 `O(1)`이 된다.



