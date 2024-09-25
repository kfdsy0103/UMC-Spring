## 자바의 접근제어자 4가지
1. public
- 모든 클래스에서 접근 가능하다.
- 아래 코드에서 A 클래스와 name, method는 어느 클래스에서든 접근 가능하다.
```java
public class A {
    public String name;

    public void method() {
        ...
    }
}
```

2. private
- 해당 클래스 내부에서만 접근 가능하다.
- 아래 코드에서 A 클래스만 외부 접근 가능, name과 method는 외부 접근 불가능
```java
public class A {
    private String name;

    private void method() {
        ...
    }
}
```

3. protected
- 같은 패키지 내 혹은 상속받은 클래스에서만 접근 가능하다.
- 아래 코드에서 A를 상속받은 B클래스는 A의 protected 속성을 이용할 수 있다.
```java
public class A {
    protected String name;

    protected void method() {
        ...
    }
}

// 다른 패키지에 존재하는 클래스이다.
public class B extends A {
    public void printName() {
        System.out.println(name); // 다른 패키지이나 상속을 받았으므로 문제없다.
    }
}
```

4. default
- 접근 제어자를 명시하지 않은 경우 default, 같은 패키지에서 접근 가능하다.
- 아래 코드에서 A 클래스와 name, method는 default이므로 같은 패키지에서 접근할 수 있다.
```java
class A {
    String name;

    void method() {
        ...
    }
}
```

## 클래스와 인터페이스
- 클래스
  - 필드와 메서드로 구성되어있어 객체를 생성할 수 있는 틀이다.
```java
public class A {
    public String name; // 필드
    public void method() { // 메서드
        ...
    }
}
```
- 인터페이스
  - 클래스에서 구현해야하는 기능을 정의하여 명세서 역할을 한다.
  - 메서드에는 기본적으로 public 접근 제어자가 붙는다.
```java
public interface A {
    void method();  // 추상 메서드
}

public class B implements A {
    @Override
    public void method() {
        ...
    }
}
```
## 상속
- 기존의 클래스의 필드와 메서드를 물려받아 새로운 클래스를 정의하는 것이다.
- 장점
  - 재사용성 : 동일한 코드를 중복 작성하지 않아도 된다.
    유지보수 : 변경사항은 부모 클래스만 수정하여 처리할 수 있다.
    다형성: 부모 타입으로 자식 객체를 처리할 수 있다.
```java
public class Parent {
    public String name;
    public int age;
    public void method() {
        ...
    }
}

public class Child extends Parent {
    // name, age, method를 모두 물려받음
    // 필드와 메서드를 추가할 수 있다.
}
```

## 면접 질문
- Optional 클래스에 대한 설명
    - Optional 클래스는 값이 존재하지 않는 NPE 상황을 명시적으로 처리할 수 있게 해주는 Wrapper 클래스이다.
    - Optional 메서드를 사용하여 값의 존재 여부를 체크하고 값이 없는 경우에 대한 처리를 정의하며 사용한다.