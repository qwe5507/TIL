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
    public static void println() {
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
    private final static String STEP_STRING = "-";
    
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
- 외부의 MoveStrategy를 구현한 RandomMoveStrategy가 랜덤한 값을 주입해주므로 테스트가 용이해진다.

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

## 2. 로또 - TDD
- [🚀 1단계 - 문자열 계산기](https://github.com/next-step/java-lotto/pull/3326)
- [🚀 2단계 - 로또(자동)](https://github.com/next-step/java-lotto/pull/3338)
- [🚀 3단계 - 로또(2등)](https://github.com/next-step/java-lotto/pull/3373)
- [🚀 4단계 - 로또(수동)](https://github.com/next-step/java-lotto/pull/3410)

  


