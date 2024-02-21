# OOP(Object Oriented Programming)
객체지향 프로그래밍  

## 용어

객체
- 사물과 같이 유형적인 것과 개념이나 논리와 같은 무형적인 것들
- 지향: 작정하거나 지정한 방향으로 나아감
- 객체 모델링: 현실세계의 객체를 SW객체로 설계하는 것

1. 객체 단위로 나누어서(조직화)
2. 객체 간의 상호작용으로 SW를 설계, 구현

class
- 객체를 만드는 설계도

instance
- 클래스를 통해 생성된 객체

interface
- 객체 간의 상호작용을 정의하는 일종의 계약서
- 인터페이스는 객체가 어떤 메소드를 가지고 있는 지 정의하고, 인터페이스를 상속받은 클래스는 만드시 해당 메소드를 구현해야 함
- 객체 간의 결합도를 낮추고 고드의 재사용성과 유지보수성과 관련  

[interface vs abstract class](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4-vs-%EC%B6%94%EC%83%81%ED%81%B4%EB%9E%98%EC%8A%A4-%EC%B0%A8%EC%9D%B4%EC%A0%90-%EC%99%84%EB%B2%BD-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)  
나중에 정리하기


module
- 프로그램을 구성하는 구성 요소로, 관련된 데이터와 함수를 하나로 묶은 단위를 의미
- 보통 하나의 소스 파일에 모든 함수를 작성하지 않고, 함수의 기능별로 따로 모듈을 구성

![module vs function](https://losskatsu.github.io/assets/images/programming/function-module-package/03.JPG)


## 객체지향 프로그래밍의 특징
- Abstraction(추상화)
    - 객체에서 중요한 부분을 강조하고, 불필요한 부분을 무시하는 것을 의미
    - 추상화를 통해 객체를 단순화하고, 복잡성을 낮출 수 있음
    - 코드의 가독성과 유지보수성과 관련
- Polymorphism(다형성)
    - 같은 이름의 메소드나 연산자가 다른 객체에 의해 다르게 구현될 수 있는 것을 의미
    - 코드의 유연성과 재사용성과 관련
    - [오버로딩 vs 오버라이딩](overloading_vs_overriding.md)
- Inheritance(상속)
    - 기존 클래스를 재활용하여 새로운 클래스를 만드는 것
    - 기존 클래스의 기능을 확장하거나 변경
    - 코드의 재사용성과 관련
- Encapsulation(캡슐화)
    - 객체가 내부 정보를 외부에 노출하지 않음
    - 내부 구현을 보호하고 객체 간의 결합도를 낮출 수 있음
    - [결합도와 응집도](./coupling_vs_cohension.md)

## [객체지향 프로그래밍의 원칙](./SOLID.md)


## 객체지향 프로그래밍의 장점
- 모듈화된 프로그래밍
- 재사용성이 높음
- 디버깅이 용이
- 정보 보호

### 출처

[BatStudio](https://www.ibatstudio.com/%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-object-oriented-programming-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0/)  