## 함수형 인터페이스
- 추상 메서드가 단 하나만 정의된 인터페이스를 의미한다.
- 람다식을 사용한 함수형 프로그래밍을 구현하기 위해 사용한다.
- 자바에서는 주로 java.util.function 패키지로 제공되는 표준 API를 사용한다.


- 종류
  - | 인터페이스       | 메서드                  | 설명                     |
    |-------------|----------------------|------------------------|
    | Runnable    | void run()           | () -> void             |
    | Consumer<T> | void accept(T t)     | T -> void              |
    | Supplier<T> | T get()              | () -> T                |
    | Function<T, R> | R apply(T t)         | T -> R                 |
    | Predicate<T> | boolean test(T t)    | T -> boolean           |
    | Operator | T applyXXX(T t, T t) | T -> T | 

    
- 사용 예시
```java
public static void main(String[] args) {

    // 수행할 작업 정의
    Runnable runnable = () -> System.out.println("Runnable 실행");
    runnable.run();

    // 입력 값을 소비
    Consumer<String> consumer = (message) -> System.out.println("Consumer 실행 : " + message);
    consumer.accept("Hello World!");
    
    // 값 생성 및 제공
    Supplier<Double> supplier = () -> Math.random();
    System.out.println("Supplier 실행 : " + supplier.get());

    // return "Function 실행 : 5", 변환 작업에 사용
    Function<Integer, String> function = (num) -> "Function 실행 : " + num;
    System.out.println(function.apply(5));

    // 조건 검사에 사용
    Predicate<String> predicate = (str) -> str.isEmpty();
    System.out.println("Predicate 실행, 빈 문자열 인가? : " + predicate.test(""));

    // UnaryOperator : 하나의 인자
    UnaryOperator<Integer> unaryOperator = (x) -> x * x;
    System.out.println("UnaryOperator 실행, 4의 제곱은 : " + unaryOperator.apply(4));

    // BinaryOperator : 두 개의 인자
    BinaryOperator<Integer> binaryOperator = (a, b) -> a + b;
    System.out.println("BinaryOperator 실행, 3과 5의 합은 : " + binaryOperator.apply(3, 5));

}
```

- 연습 문제
```java
import java.util.*;
import java.util.function.*;

public class Main {
    public static void main(String[] args) {

        Scanner scan = new Scanner(System.in);

        // P1
        Function<Integer, String> P1 = new Function<Integer, String>() {
            @Override
            public String apply(Integer integer) {
                return integer.toString();
            }
        };
        System.out.print("정수를 입력해주세요 : ");
        System.out.println(P1.apply(scan.nextInt()));

        // P2
        MyFunction P2 = new MyFunction();
        System.out.print("정수를 입력해주세요 : ");
        System.out.println(P2.apply(scan.nextInt()));

        // P3
        Function<Integer, String> P3 = (integer) -> integer.toString();
        System.out.print("정수를 입력해주세요 : ");
        System.out.println(P3.apply(scan.nextInt()));

        // P4
        Function<Integer, String> P4 = String::valueOf;
        System.out.print("정수를 입력해주세요 : ");
        System.out.println(P4.apply(scan.nextInt()));
    }

    static class MyFunction implements Function<Integer, String> {
        @Override
        public String apply(Integer integer) {
            return integer.toString();
        }
    }
}
```
<hr>

## Stream API
- 컬렉션 요소에 대한 반복 처리, 필터링, 매핑, 집계 작업을 효율적으로 처리할 수 있게 해주는 API


- 특징
  - 원본 데이터를 변경하지 않는다.
  - 일회용으로 사용할 수 있다.
  - 내부 반복으로 작업을 처리해 병렬 처리가 가능하다.


- Stream API의 구성 요소
  - 스트림 생성
  - 중간 연산
  - 최종 연산


  - 스트림 생성
  ```java
  // Collection 인터페이스에는 stream() 메서드가 정의되어 있다. 

  // 배열로부터 스트림 생성
  String[] animals = {"lion", "tiger"};
  Stream animalStream = animals.stream(animals);

  // 리스트로부터 스트림 생성
  ArrayList<String> nameList = new ArrayList<>(Arrays.asList("kim","park"));
  Stream<String> nameStream = nameList.stream();

  // 원시 스트림 생성
  Stream<String> utilStream = Stream.of("a", "b", "c");
  ```

  - 중간 연산
  ```java
  // filter(필터링), map(변환), sorted(정렬), distinct(중복제거) 등의 연산
    
  List<String> names = Arrays.asList("kim", "kim", "park", "yoon");
    
  // k로 시작하는 이름만 필터링
  names.stream()
      .filter(name -> name.startsWith("k"))
      .forEach(System.out::println);
    
  // 대문자로 변환
  names.stream()
      .map(String::toUpperCase)
      .forEach(System.out::println);
    
  // 요소 정렬
  names.stream()
      .sorted()
      .forEach(System.out::println);
    
  // 중복 제거는 equals, hashCode 메서드를 재정의한다.
  names.stream()
      .distinct()
      .forEach(System.out::println);
  ```
   
  - 최종 연산
  ```java
  // forEach(처리), collect(수집), count(집계) 등의 연산
   
  List<String> names = Arrays.asList("kim", "park", "yoon");
    
  // forEach
  name.stream().forEach(System.out::println);
   
  // collect
  List<String> upperNames = name.stream()
                                .map(String::toUpperCase)
                                .collect(Collectors.toList());
   
  // count
  Long count = names.stream().count();
  ```
    
   
- 연습 문제
```java
import java.util.*;
import java.util.stream.Collectors;

public class Main2 {
    public static void main(String[] args) {

        List<Integer> integerList = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10));

        // P1
        List<Integer> P1 = integerList.stream()
                .map(integer -> integer * 2)
                .collect(Collectors.toList());

        integerList
                .forEach(integer -> System.out.print(integer + "\t"));
        System.out.println();

        P1
                .forEach(integer -> System.out.print(integer + "\t"));
        System.out.println();

        // P2
        integerList.stream()
                .filter(integer -> integer % 2 == 0)
                .map(integer -> integer + " is even number")
                .forEach(System.out::println);

    }
}
```

## 예상 질문
- Stream과 Collection의 차이점 (연산 관점)
  - 컬렉션은 요소에 대한 연산을 즉시 수행하지만, 스트림은 최종연산(collect, forEach)이 호출될 때 까지 중간 연산(filter, map)을 지연시켜 최적화된 성능을 제공한다.