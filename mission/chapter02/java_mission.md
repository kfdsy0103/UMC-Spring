## static & jvm메모리 & classloader
### static
  - 클래스 자체에 속하는 변수/메서드를 정의할 때 사용한다.
  - 클래스 레벨에서 공유되어 인스턴스 없이 접근할 수 있다.
  - static 메서드 내부에서는 인스턴스 변수를 사용할 수 없다.
    - static은 인스턴스 생성 전에 존재하기 때문이다.
  ```java
  // 1. 변수에 사용
  public class A {
      public static int num = 0; // 공통된 값 공유
  }
  
  
  // 2. 메서드에 사용
  public class A {
      public static void main(String[] args) {
        ...
      }
  }
  
  
  // 3. 정적 변수 초기화 블록에 사용
  public class DBConnection {
    public static String connection;
    
    static {
      connection = "Connected to database";
      System.out.println(connection);
    }
  }
  
  
  // 4. 내부 클래스에 사용
  public class Outer {
    public static class Inner {
      public static void method() {
        ...
      }
    }
  }
  ```
### JVM 메모리
  - JVM(Java Virtual Machine)은 자바 프로그램이 OS로 부터 독립적으로 실행할 수 있도록 해주는 실행 환경이다.
  - JVM은 운영체제로부터 할당받은 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.
  - 메모리 공간은 크게 Method 영역 / Heap 영역 / Stack 영역으로 구분된다.

  - Method(Static)
    - 클래스가 로딩될 때 생성되는 공간이다.
    - 클래스의 정보와 Static 요소를 저장한다.
    - 프로그램 종료되기 전까지 존재한다.
    - 모든 스레드에 공유된다.
  
  - Stack
    - 메서드 내에서 정의되는 지역변수, 매개변수 값을 저장한다.
    - 각 스레드마다 독립적인 Stack 영역을 가진다.
    - 메서드가 호출되면 Stack 영역에 하나의 Stack Frame이 생성되고 메서드가 종료되면 제거된다.

  - Heap
    - 인스턴스가 저장되는 공간이다.(new)
    - 모든 스레드에 공유된다.
    - 어떤 변수도 인스턴스를 참조하지 않는다면 가비지 컬렉터에 의해 제거된다.

### ClassLoader
  - JVM의 구성요소 중 하나로, .class 파일을 읽어 JVM 메모리에 로드하는 역할을 한다.
  - 클래스 로딩 시점에 static 요소도 Method 영역에 로드되기 때문에 인스턴스 없이 사용 가능한 것이다.

## Collection interface
- 데이터를 그룹으로 묶어 다룰 수 있도록 메서드를 추상화한 인터페이스

- Collection의 하위 인터페이스와 구현체
  - List(순서O, 중복O)
    - LinkedList
    - Vector
    - ArrayList
  - Set(순서X, 중복X)
    - HashSet
    - TreeSet
  - Queue(순서O, 중복O)
    - LinkedList
    - PriorityQueue

- Map(순서X, 키 중복X)
  - Collection의 하위 인터페이스 X
  - HashTable
  - HashMap

## 예상 면접 질문
- JVM 메모리 구조에서 Heap 영역과 Stack 영역의 차이점
  - Heap은 레퍼런스 타입의 인스턴스가 저장되는 공간이다. Stack은 Heap에 대한 참조 값, 지역변수와 매개변수가 저장된다.
  - Heap은 하나만 생성되고 모든 스레드에서 공유한다. Stack은 스레드 별로 독립된 영역을 가져 공유되지 않는다.
  - Heap은 어떤 변수에도 참조되지 않을 때 GC의 제거 대상이 된다. Stack은 메서드가 종료되면 해당 스택이 제거된다.