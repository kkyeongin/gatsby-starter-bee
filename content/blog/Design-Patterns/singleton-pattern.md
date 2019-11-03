---
title: Singleton Pattern
date: 2019-02-19 20:02:95
category: Design-Patterns
---

# 싱글턴 패턴

![singleton.png](https://realzero0.github.io/assets/img/singletonpattern.png)

## 디자인 패턴이란 무엇인가?

> 어떤 상황의 문제에 대한 해법

패턴의 4가지 요소

* 이름
* 문제
* 해법
  * 설계를 구성하는 요소들과 그 요소들 간과 관계, 책임, 그리고 협력 관계
* 결과 (장, 단점)
  
## Singleton(단일체)

> 어떤 클래스의 인스턴스는 오직 하나임을 보장하며, 이 인스턴스에 접근할 수 있는 전역적인 접촉점을 제공하는 패턴

시스템 관리자, 윈도우 관리자 오직 하나여야 하듯이 클래스 자신이 자기의 유일한 인스턴스로 접근하여 자체적으로 관리한다.

이 클래스는 또 다른 인스턴스가 생성되지 않도록 할 수 있고, 클래스가 인스턴스에 대한 접근 방법을 제공할 수 있다.

### 장점

1. 유일한 인스턴스의 접근 제어 가능
2. 연산 및 표현의 정제 허용
   * 싱글턴 클래스를 상속하여 서브클래스를 통해 새로운 인스턴스를 만들 수 있다.  
3. 싱글톤으로 만들어진 클래스의 인스턴스는 전역 인스턴스이기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하기 쉽다.

```cpp
class Singleton{
public:
    static Sington* Instance();
protected:
    Singleton();
    // 생성자의 외부 접근을 차단함
private:
    static Sigleton* instance_;
};

=====================================

Singleton* Singleton::instance_ = 0;

Singleton* Singleton::Instance () {
    if (_instance == 0) {
        _instance = new Singleton;
    }
    return _instance;
}

```

### ex. Thread Safe Singleton

```java

public class ThreadSafeSingleton {

    private static ThreadSafeSingleton instance;

    private ThreadSafeSingleton(){}

    public static synchronized ThreadSafeSingleton getInstance()
    {
        if(instance == null){
            instance = new ThreadSafeSingleton();
        }
        return instance;
    }
}
```

synchronized 때문에 메소드에 접근을 쓰레드가 동시 접근이 안돼, thread-safe 할 수 있다. 하지만 쓰레드들의 getInstance() method 를 호출하게 되면 높은 cost 비용으로 인해 프로그램 전반에 성능저하가 일어난다.

### ex. Enum Singleton

```java

public enum EnumSingleton {

    INSTANCE;
    
    public static void doSomething(){
        //do something
    }
}
```

enum은 전역 접근이 가능해, 싱글턴 객체 역시 접근 가능 하다.
단점으로는 enum type은 값변경이 불가능해 lazy initialization이 허용이 안된다.

### ex. Serialization and Singleton

분산 시스템에서 Singleton 클래스에 Serializable 인터페이스를 구현하여, 파일 시스템에 상태를 저장하고 나중에 검색 할 수 있도록 해야 한다. 

```java
import java.io.Serializable;

public class SerializedSingleton implements Serializable{

    private static final long serialVersionUID = -7604766932017737115L;

    private SerializedSingleton(){}
    
    private static class SingletonHelper{
        private static final SerializedSingleton instance = new SerializedSingleton();
    }
    
    public static SerializedSingleton getInstance(){
        return SingletonHelper.instance;
    }
    
}
```
문제점은 역 직렬화 할 때마다 클래스의 새로운 인스턴스를 생성한다는 것이다.


### ex. initialization on demand holder idiom
미국 메릴랜드 대학의 컴퓨터 과학 연구원인 Bill pugh 가 기존의 java singleton pattern이 가지고 있는 문제들을 해결 하기 위해 새로운 singleton pattern을 제시하였다. Initialization on demand holder idiom기법이다. 이것은 jvm 의 class loader의 매커니즘과 class의 load 시점을 이용하여 내부 class를 생성시킴으로 thread 간의 동기화 문제를 해결한다.

```java
public class InitializationOnDemandHolderIdiom {
	
	private InitializationOnDemandHolderIdiom () {}
	private static class Singleton {
		private static final InitializationOnDemandHolderIdiom instance = new InitializationOnDemandHolderIdiom();
	}
	
	public static InitializationOnDemandHolderIdiom getInstance () {
		System.out.println("create instance");
		return Singleton.instance;
	}
}
```

 lazy initialization이 가능하며 모든 java 버젼과, jvm에서 사용이 가능하다. 현재 java 에서 singleton 을 생성시킨다고 하면 거의 위의 방법을 사용한다고 보면 된다.

### 참고

* [Java Singleton Design Pattern Best Practices with Examples](https://www.journaldev.com/1377/java-singleton-design-pattern-best-practices-examples#thread-safe-singleton)
* [seotory님의 블로그(싱글턴 패턴)]([https://jeong-pro.tistory.com/86](https://blog.seotory.com/post/2016/03/java-singleton-pattern))
* [singleton diagram picture](https://realzero0.github.io/study/2017/06/12/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%A0%95%EB%A6%AC.html)