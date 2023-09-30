
# JDK 11 vs JDK 17

주로 사용하는 Java 11에서 Java 17로 마이그레이션 하면서, 주요 특징에 대해 정리한다. 

## Java 11의 주요 특징 
- **LTS**
    - JDK 8 보다 LTS 기간이 짧기 때문에, JDK 17 고려
- **G1 GC가 기본 GC로 설정**
- **var 타입(JDK 10)**
  - 로컬 변수 한정으로 타입을 추론
  - JDK11에서는 람다식에서 사용 가능
  - ```java
    var a = 10; //값을보고 int 타입 추론
    var result = new ArrayList<Integer>();
    ```
- **String 메소드**
  - isBlank(), repeat(), strip()
- **Files.writeString() / Files.readString()**
  - 문자열을 통으로 저장하고 읽는 기능

## Java 17의 주요 특징 
- **LTS**
- **ZGC 도입**
  - default는 G1GC
- **switch 식(JDK14)**
  - switch 문법이 값을 가지는 식으로 사용가능
  - 변수에 할당 가능
  - 값을 지정하기 때문에, 모든 경우의 수의 값을 지정하던지, default문을 반드시 기입해야 한다.
  - ```java
    String grade = switch (mark) {
    case 10 -> "A";
    case 9 -> "B";
    case 8 -> "C";
    case 7 -> "D";
    default -> "F";
    }
    ```
- **텍스트 블록(JDK15)**
  - 타 언어와 마찬가지로, 여러 줄을 입력할 수 있는 기능 추가
  - `""" """` 
- **stream 개선 (JDK16)**
  - `toList()` 메소드 추가
    - stream List 반환 시 결과 메소드 `.collect(Collectors.toList())`로 마무리 할 필요 없이 `toList()`를 사용
    - ```java
        List<Integer> nos1 = Stream.of(1, 2, 3).filter(n -> n % 2 ==0).toList();
        ```
  - `mapMulti()` 메소드 추가
    - 하나의 값을 받아, 여러 값을 추출
    - ```java
        List<Integer> result = Stream.of(1, 2, 3)
        .mapMulti((Integer no, Consumer<Integer> consumer) -> {
          consumer.accept(no);
          consumer.accept(-no);
        }).toList();
        
        //[1, -1, 2, -2, 3, -3]
        ```
- **String format 개선 (JDK15)** 
  - 문자열에서 .formatted() 메소드로 바로 포매팅 기능 지원
- **NPE 메시지 개선 (JDK15)**
  - NPE 오류 위치를 더 상세히 알려준다.
- **instaceof와 패턴 매칭 (JDK16)**
  - ```java
    if(a instanceof String s){
	    System.out.println(s);
    }
    
    if(a instanceof String t && t.isBlank()){
        System.out.println("blank");
    }
    
    if(!(a instanceof String b)){
        return; 
    }
        b.toUpperCase(Locale.ROOT
    ```
    - instanceof 후에 조건식에서 캐스팅된 변수를 바로 사용할 수 있다.
    - 위의 예시에서 s를 패턴변수라고 한다.
    - 3번째 조건의 경우 조건식 밖에서 사용할 수 있다.
- **record클래스**
  - ```java
      public record FullName(String first, String last) {

      }
      
      FullName fn = new FullName("f","l");
      fn.first();
      fn.last();
    ```
  first, last 변수를 가지는 데이터 클래스
  - 모든 필드가 private final
  - 파라미터를 가진 생성자
  - 같은 이름의 메서드(getter) ->(first(), last())
  - hashCode, equals, toString 구현
 
  특징
  - 기본 생성자 없음
  - 값 변경 메서드 없음
  - final 클래스 (추상 클래스 아님)
  - 다른 클래스 상속 불가

- **sealed 타입**
  - 상속할 수 있는 타입을 제한
  - ```java
    public sealed interface FList<T> permits Cons, Empty {}

    final class Cons<T> implements FList<T>{
        private T head;
        private FList<T> tail;
    
        public Cons(T head, FList<T> tail) {
            this.head = head;
            this.tail = tail;
        }
    }
    ```
  - `sealed` : class나 interface를 실드 타입으로 지정 
  - `permits` : 상속을 허용하는 타입 목록 지정, 하위 타입이 존재해야 한다.
  - sealed타입을 상속한 타입은 3가지중 하나를 지정해야 한다.
    - final
    - sealed (+permits)
    - non-sealed (제한하지 않음)
  - sealed 타입을 상속한 타입은 같은 패키지에 위치해야 한다.
    - 또는 같은 모듈에 위치해야 한다.(named module)
  - 같은 java 파일에 위치하면 permits 키워드를 생략 할 수 있다.
  - record 타입도 sealed 인터페이스를 상속할 수 있다.


## Reference
- https://www.youtube.com/watch?v=7SlDdzVk6GE&t=23s
- https://velog.io/@jinyeong-afk/기술-면접-Java-11-vs-Java-17-차이
- https://velog.io/@ililil9482/Java17을-고려해야할까#-java-16