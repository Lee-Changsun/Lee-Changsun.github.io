--- 
layout: post
title: "디자인 패턴(Design Pattern) 템플릿 메서드 패턴(Template Method Pattern)"
subtitle: '싱글턴 패턴이 무엇이고, 어떠한 특징을 가지는지'
author: "window_for_sun"
header-style: text
tags:
    - 디자인 패턴
    - Design Pattern
    - Template Method Pattern
---  
[참고](/_posts/2019-02-08-designpattern-intro.md)  

### 템플릿 메서드 패턴이란
- 어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴
    - 전체적으로는 동일하면서 부분적으로는 다른 구문으로 구성된 메서드의 코드 중복을 최소화 할 때 유용하다.
    - 다른 관점에서 보면 동일한 기능을 상위 클래스에서 정의하면서 확장/변화가 필요한 부분만 서브 클래스에서 구현할 수 있도록 한다.
    - 예를 들어, 전체적인 알고리즘은 상위 클래스에서 구현하면서 다른 부분은 하위 클래스에서 구현할 수 있도록 함으로써 전체적인 알고리즘 코드를 재사용하는 데 유용하도록 한다.
- 행위(Behavioral) 패턴 중 하나
- ![템플릿 메서드 클래스 다이어그램 예시](/../img/designpattern-templatemethod-ex-classdiagram.png)
<!--
@startuml
skinparam classAttributeIconSize 0
abstract class AbstractClass {
    + templateMethod()
    # primitiveOperation1()
    # primitiveOperation2()
}

class ConcreteClass {
    # primitiveOperation1()
    # primitiveOperation2()
}

AbstractClass <|-- ConcreteClass
@enduml
-->
- 역할이 수행하는 작업
    - AbstractClass
        - 템플릿 메서드를 정의하는 클래스
        - 하위 클래스에 공통 알고리즘을 정의하고 하위 클래스에서 구현될 기능을 primitive 메서드 또는 hook 메서드로 정의하는 클래스
    - ConcreteClass
        - 물려받은 primitive 메서드 또는 hook 메서드를 구현하는 클래스
        - 상위 클래스에 구현된 템플릿 메서드의 일반적인 알고리즘에서 하위 클레스에 적합하게 primitive 메서드나 hook 메서드를 오버라이드하는 클래스
        
### 예시
#### 여러 회사의 모터 지원하기
- ![모터 클래스 다이어그램](/../img/designpattern-templatemethod-motor-1-classdiagram.png)
<!--
@startuml
skinparam classAttributeIconSize 0
class HyundaiMotor {
    - door : Door
    - motorStatus : MotorStatus
    
    + HyundaiMotor(door : Door)
    + move(direction : Direction) : void
    - moveHyundaiMotor(direction : Direction) : void
    + getMotorStatus() : MotorStatus
    - setMotorStatus(motorStatus : MotorStatus) : void
}

class Door {
    - doorStatus : DoorStatus
    + Door()
    + getDoorStatus() : DoorStatus
    + open() : void
    + close() : void
}

enum MotorStatus {
    + MOVING
    + STOPPED
}

enum DoorStatus {
    + OPENED
    + CLOSE
}

enum Direction {
    + UP
    + DOWN
}

HyundaiMotor o-- Door
HyundaiMotor *-- MotorStatus
HyundaiMotor o-- Direction
Door *-- DoorStatus
@enduml
-->
- 엘리베이터 제어 시스템에서 모터를 구동시키는 기능
    - 예를 들어 현대 모터를 이용하는 제어 시스템이라면 HyundaoMotor 클래스에 move 메서드를 정의할 수 있다.