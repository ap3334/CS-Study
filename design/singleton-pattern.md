### Singleton Pattern

---

**싱글톤 패턴이 무엇인가요?**

하나의 클래스에 오직 하나의 인스턴스만을 가지는 패턴이다.

<img width="306" alt="싱글톤 패턴" src="https://user-images.githubusercontent.com/62919440/219193234-5b27c2d6-8496-4a9e-9e76-bdc574371b83.png">

<br>

**싱글톤 패턴의 장점?**

하나의 인스턴스를 공유하여 사용하므로 인스턴스를 생성할 때 드는 비용이 줄어듭니다.

<br>

**싱글톤 패턴의 단점?**

- 의존 관계상 클라이언트가 구체 클래스에 의존한다 → DIP 위반
- 단위 테스트를하기 어렵다 → 독립적인 인스턴스를 만들기가 어려움

```java
public class Singleton {
  
  private static Singleton instance;
  
  private Singleton() {} // 생성자는 private을 통해 막아줌
  
  public static Singleton getInstance() {
    
    if (instance == null) {
      instance = new Singleton(); // instance가 존재하지 않다면 생성
    }
    
    return instance; // 존재한다면 기존거 사용
  }
  
}
```

**이렇게 구현하는 방법의 문제점**

멀티스레드 환경에서 안전하지 않음 → 먼저 if문을 통과한 쓰레드가 아직 instance를 생성하기 전에 다른 쓰레드가 if문 검사를 하게 된다면 두 쓰레드는 다른 instance를 가진다

**멀티쓰레드 환경에서 안전한 싱글톤 코드 4가지**

1. synchronized 키워드

```java
public class Singleton {
  
  private static Singleton instance;
  
  private Singleton() {} 
  
  public static synchronized Singleton getInstance() { // 해당 메서드에 한 번에 하나의 쓰레드만 들어옴
    
    if (instance == null) {
      instance = new Singleton();
    }
    
    return instance;
  }
  
}
```

> 동시에 여러 쓰레드가 메서드에 들어갈 수 없기 때문에 하나의 인스턴스만 보장할 수 있다.
> 하지만 getInstance() 호출 시 동기화 처리하는 방법때문에 성능의 저하가 있을 수 있다.

2. eager initialization

```java
public class Singleton {
  
  private static final Singleton INSTANCE = new Singleton(); // 미리 만들어놓음
  
  private Singleton() {} 
  
  public static synchronized Singleton getInstance() {
    
    return INSTANCE;
  }
  
}
```

> 클래스가 로딩되는 시점에 필드들이 초기화되기 때문에 미리 instance가 만들어짐
> 하지만 미리 만들어놓는 점이 단점이 될 수 있음
> instance를 만드는 시간이 오래 걸린다면 사용하지 않았을 때 낭비일 수 있음

3. double checked locking

```java
public class Singleton {
  
  private static volatile Singleton instance; 
  
  private Singleton() {} 
  
  public static Singleton getInstance() {
    
    if (instance == null) {
    
      synchronized (Singleton.class) {
      
        if (instance == null) { // instance가 null인지를 두 번 체크함
        
          instance = new Singleton();
          
        }
        
      }
      
    }
    
    return instance;
  }
  
}
```

> 메서드 호출시 매번 synchronized가 걸리는 것이 아닌 아주 가끔 동시의 쓰레드가 if문 안에 들어올 때만 synchronized가 걸리기 때문에 성능에 유리
> 필요한 시점에 인스턴스를 만들 수 있음
> 자바 1.5이상에서만 동작

4. static inner class(권장)

```java
public class Singleton {
  
  private Singleton() {} 
  
  private static class SingletonHolder {
  
    private static final Singleton INSTANCE = new Singleton();
  
  }
  
  public static synchronized Singleton getInstance() {
    
    return SingletonHolder.INSTANCE;
  }
  
}
```

  
