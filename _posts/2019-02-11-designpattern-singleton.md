author: "window_for_sun"
header-style: text
tags:
    - 디자인 패턴
    - Design Pattern
    - Singleton
---
[참고](/_posts/2019-02-08-designpattern-intro.md)  

### 싱글턴 패턴이란
- 전역 변수를 사용하지 않고 객체를 하나만 생성 하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 패턴
- [싱글턴 클래스 다이어그램 예시](/img/designpattern-singleton-ex-classdiagram.png)
- 하나의 인스턴스만을 생성하는 책임이 있으며 getInstance() 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환하는 작업을 수행한다.

### 예시
#### 프린터 관리자 만들기
- 동일한 하나의 프린트를 10명이 공유해서 사용한다고 가정하자.
- 아래는 일반적인 프린트 클래스다.
```java
public class Printer {
    public Printer() {}     // @1
    
    public void print(String str){      // @2
        System.out.println(str);
    }
}
```
- 위의 상황에서 하나의 프린트를 여러명이 공유하기 때문에 여러명의 클라이언트가 하나의 객채를 사용하는 상황이다.
- 이러한 프린트 클래스를 싱클턴으로 작성하면 아래와 같다.
```java
public class PrinterSingleton {
    private static PrinterSingleton instance = null;    // @3
    
    private PrinterSingleton(){}        // @1
    
    public static PrinterSingleton getInstance(){   // @4
        if(instance == null){
            instance = new PrinterSingleton();
        }
        
        return instance;
    }
    
    public void print(String str) {     // @2
        System.out.println(str);
    }    
}
```
#### 비교 및 분석
- Printer 클래스
    - 생성자, 출력 함수로 간단하게 구성되어 있다.
    - new Printer()(생성자)를 통해 객체를 생성하고, print() 함수를 호출해서 사용해야한다.
    - 즉 여러명이 사용 할 경우 모두 생성자를 통해 프린트 객체를 생성하고 출력함수를 호출하게 된다.
    - 프린트는 정작 1개지만 여러개의 여러개의 프린트를 사용 중인 상황이 되버린다.
- PrintSingleton 클래스
    - Printer 클래스와 비교해서 @3, @4가 추가 되어, static 프로퍼티, private 생성자, static 메서드, 출력 함수 로 구성되어 있다.
        1. @3 static 프로퍼티는 자신의 인스턴스를 가진다.
        1. @4 private 생성자는 외부로 부터 생성자 호출을 통한 인스턴스 생성를 못하도록 한다.
        1. @4 static 메서드는 외부로 부터 자신의 인스턴스를 리턴 한다.
        1. @2 출력 함수는 인스턴스로 부터 호출되어 프린트 동작을 수행한다.
    - 사용자들이 프린트를 사용하기 위해서는 getInstance를 호출하여 인스턴스를 리턴 받아 사용하게 된다.
    - getInstance() 메서드에서는 초기에만(== null) 인스턴스를 생성하고 이후에는 계속해서 재사용 하기때문에, 하나라는 조건과 동일하는 조건이 만족된다.
    
#### 사용
- Print 클래스 
