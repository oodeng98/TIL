# Overloading vs Overriding

## Overloading이란?
같은 이름의 메소드를 여러 개 정의하고, 매개변수의 타입, 개수, 순서 등을 다르게 하여 구현하는 것을 의미  
오버로딩을 통해 같은 이름의 메소드를 다양한 상황에서 사용할 수 있으며, 코드의 가독성과 유지보수성을 높일 수 있음
```java
class OverloadingTest{
    public static void main(String[] args) {
        OverloadingMethods om = new OverloadingMethods();

        om.print();
        System.out.println(om.print(3));
        om.print("Hello!");
        System.out.println(om.print(4, 5));

    }
}

class OverloadingMethods{
    public void print(){
        System.out.println("Overloading 1");
    }
    String print(Integer a){
        System.out.println("Overloading 2");
        return a.toString();
    }
    void print(String a){
        System.out.println("Overloading 3");
        System.out.println(a);
    }
    String print(Integer a, Integer b){
        System.out.println("Overloading 4");
        return a.toString() + b.toString();
    }
}
/*
Overloading 1
Overloading 2
3
Overloading 3
Hello!       
Overloading 4
45
*/
```

## Overriding이란?
부모 클래스에서 상속받은 메소드를 자식 클래스에서 새롭게 정의하고 구현하는 것을 의미  

```java
class OverridingTest{
    public static void main(String[] args) {
        Person person = new Person();
        Child child = new Child();
        Senior senior = new Senior();

        person.cry();
        child.cry();
        senior.cry();
    }
}

class Person{
    void cry(){
        System.out.println("흑흑");
    }
}

class Child extends Person{
    @Override
    protected void cry(){
        System.out.println("잉잉");
    }
}

class Senior extends Person{
    @Override
    public void cry(){
        System.out.println("훌쩍훌쩍");
    }
}
/*
흑흑
잉잉    
훌쩍훌쩍
*/
```
*overload: 초과 적재  
-> 같은 이름을 가지는 메서드를 많이 담아서 overload라는 이름을 사용  
override: 우세하다, 우월하다
-> 기존에 존재하는 메서드를 무시하고 새로운 메서드를 정의하는 것이므로 overide라는 이름 사용* 

### 출처
[heyhyo](https://hyoje420.tistory.com/14)