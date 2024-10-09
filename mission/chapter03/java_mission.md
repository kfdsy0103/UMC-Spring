## 제네릭과 와일드카드
### 제네릭
- 클래스나 메소드에서 사용할 내부 데이터 타입을 사용자가 지정하는 것
  - 제네릭 타입 : 제네릭을 이용한 클래스와 인터페이스
  - 제네릭 메서드 : 제네릭을 이용한 메서드
  - 타입 파라미터 : 제네릭 타입과 메서드가 가지고 있는 결정되지 않은 타입
  
- 장점
  - 컴파일 타임에 타입을 체크하므로 런타임에 발생할 수 있는 오류를 줄여준다.
  - 여러 타입에 대해 동일한 코드를 사용하므로 유지보수성이 높아진다.
  
```java
// 타입을 파라미터로 받아 Box를 생성한다.
public class GenericBox<T> {
    private T item;
    
    public void setItem(T item) {
        this.item = item;
    }
    
    public T getItem() {
        return item;
    }
}

public class Main {
    public static void main(String[] args) {
        GenericBox<String> stringBox = new GenericBox<>();
        stringBox.setItem("Hello");
        System.out.println(stringBox.getItem());
    }
}
```

### 와일드카드
- 제네릭 타입의 불공변 문제를 해결하고, 타입 파라미터 범위를 제한하기 위해 사용하는 기능
  - 종류
    - 비 한정적 와일드카드
      - 어떤 타입이든 받을 수 있다.
      - 타입이 제한되지 않기 때문에 활용이 제한적이다.
      ```java
      public void printBox(GenericBox<?> box) {
          System.out.println(box.getItem());
          box.setItem("Hello"); // 오류 발생: 어떤 타입인지 모르기 때문에 값을 설정할 수 없다.
      }
      ```
    - 상한 경계 와일드카드
      - 특정 타입의 하위 클래스들만 받는다.
      ```java
      public class Main {  
    
          // Number와 하위 클래스만 들어올 수 있다.
          public void printNumbers(List<? extends Number> list) {
              for (Number num : list) {
                  System.out.println(num);
              }
          }
    
          public static void main(String[] args) {
              List<Integer> integers = Arrays.asList(1, 2, 3);
              printfNumbers(integers);
          }
      }
      ```
    - 하한 경계 와일드카드
      - 특정 타입의 상위 클래스들만 받는다.
      ```java
       public class Main {  
    
          // Integer와 상위 클래스만 들어올 수 있다.
          public void addNumbers(List<? super Integer> list) {
              list.add(10);
          }
    
          public static void main(String[] args) {
              List<Number> numbers = new ArrayList<>();
              addNumbers(numbers);
          }
      }
      ```
  
### PECS 공식
- 와일드카드가 적용된 객체의 값을 읽을 경우 extends, 값을 쓰는 경우 super를 쓴다.

## 래퍼 클래스
- 기본 타입을 객체로 다루기 위해 사용하는 클래스이다.
```
byte -> Byte
char -> Char
int -> Integer
float -> Float
double -> Double
boolean -> Boolean
long -> Long
short -> Short
```
- 사용하는 이유
  - 컬렉션에는 기본 타입을 사용할 수 없기 때문이다.
  - 유틸리티 메서드 사용이 가능하다.
  - null 값을 가질 수 있어 DB 조회 시 null 처리에 용이하다.

## 옵셔널
- null 값을 안전하게 처리하도록 돕는 클래스이다.
```java
Optional<User> user = userRepository.findUserById(userId);

// 값이 존재한다면 출력
if(user.ifPresent()){
    System.out.println(user.getName());
} else {
    System.out.println("값이 존재하지 않습니다.");
}
```
- 사용하는 이유
  - null 처리 로직을 직관적이고 단순하게 작성할 수 있다.
  - NPE를 방지할 수 있다.

## 면접 질문
- static 멤버에서 타입 변수 T를 사용하지 못하는 이유
  - 타입 변수는 인스턴스 변수와 같이 생성 시점에 결정이 된다. 따라서 static은 인스턴스 생성 전에 존재하므로 사용할 수 없다.