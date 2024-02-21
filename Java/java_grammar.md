# 자바 기초 문법


### 출력
- print: 줄 넘김 기능 없음
- println: 출력 + 한줄(ln -> line)
- printf
  - %d: 10진수 정수(**d**ecimal)
  - %f: 실수(**f**loat)
    - %.nf: 소숫점 자릿수 n개만 남도록 반올림
  - %c: 문자(**c**har)
  - %s: 문자열(**s**tring)
  - %o: 8진수(**o**ctal)
  - %x: 16진수(He**x**adecimal)
    - 10 ~ 15를 a ~ f로 출력
      - %X: A ~ F로 출력
  - %nd: n칸을 확보한 상태로 우측에 붙여서 출력
  
- [escape 문자](./escape.md)


### 변수

#### 변수란?
- 데이터를 저장할 때 메모리의 위치를 나타내는 이름
- 메모리 상에 데이터를 보관할 수 있는 공간을 확보
- 적절한 메모리 공간을 확보하기 위해서 변수의 타입 등장

#### 변수 작명 규칙
- 대소문자 구분
- 공백은 허용되지 않음
- 숫자로 시작 불가
- $와 _는 변수이름에 사용 가능, 그 외의 특수문자는 허용하지 않음
- 예약어는 사용할 수 없다
  - 예약어: keyword, 자바 문법을 위해 미리 지정되어 있는 단어
- 합성어의 경우 주로 camelCase를 활용
- class는 PascalCase 사용
  - [네이밍 문법 설명](./naming.md)
- 한글을 이용한 변수 작명 가능, 하지만 권장하지 않는다.

#### 변수 선언 방법
```java
// 1, 변수 선언

// 자료형 변수명;
int a;

// 2, 할당(저장) =
a = 100

// 3, 초기화
int b = 300;

int n1 = 10;
int n2 = n1;
```


### Data Type

**기본 자료형**
|타입|세부타입|데이터형|크기|기본값|값의 범위|
|---|---|---|---|---|---|
|논리형||boolean||false|true/false|
|문자형||char|2byte|null|0 ~ 65,535|
|숫자형|정수형|byte|1byte|0(byte)|-128 ~ 127|
|숫자형|정수형|short|2byte|0(short)|-32,768 ~ 32,767|
|숫자형|정수형|int|4byte|0|-2,147,483,648 ~ 2,147,483,647|
|숫자형|정수형|long|8byte|0L|-9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807|
|숫자형|실수형|float|4byte|0.0f||
|숫자형|실수형|double|8byte|0.0d||
- 실수형의 경우 근사값임.
- 컴퓨터의 2진법과 인간의 10진법 간의 차이 때문
- [왜 양수는 범위가 1 작은가?](/CS/Etc/value_range.md)

```java
int max = Integer.MIN_VALUE  //int형 중 제일 작은 값
int min = Integer.MAX_VALUE  // int형 중 제일 큰 값
```
이를 최댓값, 최솟값을 구하는 것에 사용할 수 있다.

### 연산자
*모르거나 헷갈리는 내용만 적음*

#### 단항 연산자
- 증감 연산자 ++, --
  - 피연산자의 값을 1 증가, 감소시킴
  - 전위형(prefix) ++i, 먼저 값을 연산하고 그 값을 사용
  - 후위형(postfix) i--, 먼저 값을 사용하고 후에 값 연산
- 논리 부정 연산자 !
- 비트 부정 연산자 ~
- 형 변환 연산사(type)

#### 산술 연산자
정수와 정수의 연산 -> 정수
정수와 실수의 연산 -> 실수

#### 비교 연산자
- 객체 타입 비교 연산
  - instanceof
- 자바에서 문자열의 비교는 ==, !=로 하지 않음
- 문자열에서 ==, !=는 참조값을 비교
- 문자열에서 .equals는 값을 비교

#### 논리 연산자
- &&: and
- ||: or
- !: not
자바에서도 단축평가는 이루어짐
- 단축평가
  - 검사할 필요가 없다는 것을 컴파일러가 알고 뒤의 연산을 무시하는 것
  - False and True 와 같은 경우, False가 나온 순간 False가 확정되므로 뒤의 True는 고려하지 않음
  - True or False의 경우, True가 나온 순간 True가 확정되므로 뒤의 False는 고려하지 않음

#### 삼항 연산자
- 조건식 ? 식1: 식2
- 조건식이 참이면 식1 수행
- 조건식이 거짓이면 식2 수행


### 조건문
파이썬의 경우 if, elif, else구조  
자바는 if, else if, else의 구조


### 반복문
- do while문
```java
do{
  반복 수행할 문장
}while (조건식)
```
블럭 내용을 먼저 수행 후 조건식 판단(최소 한번은 수행)
그 외의 내용은 while문과 동일


### 형변환

- 문자열 -> 정수형
  - Integer.parseInt(문자열)


### 배열(Array)

- 같은 종류의 데이터를 저장하기 위한 자료구조
  - 파이썬의 경우 다른 종류의 데이터도 저장할 수 있지만, 자바의 경우 같은 종류의 데이터만 가능하다.
- 크기가 고정되어 있음
- 배열을 객체로 취급
- 배열이름.length를 통해 배열의 길이 조회 가능

#### 배열의 선언
- 타입[] 배열이름
- 타입 배열이름[]
 
|타입|배열이름|선언|
|---|---|---|
|int|iArr|int[] iArr;|
|char|cArr|char[] cArr;|
|boolean|bArr|bolean[] bArr;|
|String|strArr|String[] strArr;|
|Date|dateArr|Date[] dateArr;|

#### 배열의 생성과 초기화
- 자료형[] 배열이름 = new 자료형[길이];
- 자료형[] 배열이름 = new 자료형[] {값1, 값2, 값3};
- 자료형[] 배열이름 = {값1, 값2, 값3};
```java
int[] arr1 = new int[5];
int[] arr2 = new int[] {1, 2, 3, 4};
// int[] arr2 = new int[5] {1, 2, 3, 4} 와 같이 길이와 초기값을 동시에 지정할 순 없다
int[] arr3 = {1, 2, 3, 4};
// 선언과 동시에 할 때만 사용 가능
```

#### 배열의 순회(for-each)

- 가독성이 개선된 반복문으로, 배열 및 Collections에서 사용 가능
- index 대신 직접 요소에 접근하는 변수를 제공
- *python의 for i in interable과 같은 형식이라고 생각*
```java
int[] intArray = {1, 3, 5, 7, 9}
for(int x: intArray){
  System.out.println(x);
}
// 1
// 3
// 5
// 7
// 9
```

#### 배열의 출력
- 반복문을 통해서 출력
- Arrays.toString(배열): 배열 안의 요소를 [값1, 값2...]의 형태로 출력
- *python에서 list를 그냥 출력하면 나오는 형태로 나오는군?*
- 그냥 System.out.println(배열)을 하면 메모리의 주소값이 나옴

#### 배열의 복사
- 배열은 생성하면 길이를 변경할 수 없음
- 만약 더 많은 저장공간이 필요하다면? -> 큰 배열을 생성하고 이전 배열의 값을 복사해야함
```java
new_array = Arrays.copyOf(원본 배열, 새로운 배열의 크기)

int[] new_array = new int[10]
System.arraycopy(원본 배열, 원본 배열의 시작점, 복사 배열, 복사 배열의 시작점, 복사할 크기)
```


### 2차원 배열

#### 2차원 배열의 선언
```java
int[][] iArr;  // 권장
int iArr[][];
int[] iArr[];
```

#### 2차원 배열의 생성
```java
배열의 이름 = new 배열유형(자료형)[행의 길이][열의 길이]
배열의 이름 = new 배열유형(자료형)[행의 길이][]
```
```java
int [][] arr = {
  {1, 2, 3, 4}, 
  {5, 6, 7}, 
  {8}
};
```
위와 같이 생성해도 상관없다.


### Class 생성
클래스를 만들기 위해서는 우클릭 > new > class  
public class 클래스명 {}  
클래스명은 [PascalCase](./naming.md#camelcase)를 따름
```java
public class Person{
  String name;
  int age;
  String hobby;
}
```
```java
public class PersonTest{
  Person jung = new Person();
  jung.name = "Jung";
  jung.age = 27;
  jung.hobby = "Youtube";
}
```

### method 생성
함수란 특정한 작업을 수행하는 문장들을 한 단위로 묶은 것으로, 자바에서는 메서드(클래스 안에 정의된 함수)로 사용
```java
반환형 함수명(매개변수1, 매개변수2...){
  문장1;
  문장2;
  return 반환값;
}
public static void main(String[] args){
  System.out.println("이것은 첫번째고");
  System.out.println("이것은 두번째");
}
```

### 제어자
클래스와 클래스 멤버 선언 시 사용하여 부가적인 의미를 부여하는 키워드  
기타 제어자는 경우에 따라 여러 개를 함께 사용할 수 있지만, 접근 제어자는 2개 이상 사용할 수 없다.

#### 접근 제어자
1. private
- 선언된 클래스 멤버는 외부에 공개되지 않으며, 외부에서 직접 접근할 수 없음
- 즉, 해당 객체의 public method를 통해서만 접근할 수 있음
- 클래스 내부의 세부적인 동작을 구현하는 데 사용
![private](https://www.tcpschool.com/lectures/img_java_access_private.png)
```java
public class SameClass{
  private String var = "같은 클래스만 허용"; // private 필드
  private String getVar(){
    return this.var;
  } // private method
}
```
2. public
- 선언된 클래스 멤버는 외부에 공개되며, 해당 객체를 사용하는 프로그램 어디에서나 접근 가능
- 앞서 말했듯 public method를 통해서만 해당 객체의 private 멤버에 접근할 수 있으므로, public method는 private 멤버와 프로그램 사이의 인터페이스 역할을 수행한다고 볼 수 있음
![public](https://www.tcpschool.com/lectures/img_java_access_public.png)
```java
public class EveryWhere{
  public String var = "누구든지 허용"; // public 필드
  public String getVar(){
    return this.var;
  } // public method
}
```
3. default
- java는 클래스 및 클래스 멤버의 접근 제어 기본값으로 default 접근 제어를 별도로 명시하고 있음
- default를 위한 접근 제어자는 따로 존재하지 않고, 접근 제어자가 지정되지 않으면 자동적으로 default 접근 제어를 가지게 됨
- default 접근 제어를 가지는 멤버는 같은 클래스의 멤버와 같은 패키지에 속하는 멤버에서만 접근 가능
![default](https://www.tcpschool.com/lectures/img_java_access_default.png)
```java
public class SamePackage{
  String sameVar = "같은 패키지는 허용"; // default 필드
}
```
4. protected
- 부모 클래스에 대해서는 public 멤버처럼 취급되며, 외부에서는 private 멤버처럼 취급
클래스의 protected 멤버에 접근할 수 있는 영역
1. 이 멤버를 선언한 클래스의 멤버
2. 이 멤버를 선언한 클래스가 속한 패키지의 멤버
3. 이 멤버를 선언한 클래스를 상속받은 자식 클래스의 멤버
![protected](https://www.tcpschool.com/lectures/img_java_access_protected.png)
```java
package test;

public class SameClass{
  protected String sameVar = "다른 패키지에 속하는 자식 클래스까지 허용"; // protected 필드
}
```
```java
package test.other;
import test.SameClass; // test 패키지의 SameClass 클래스를 포함시킴

public class ChildClass extends SameClass{
  public static void main(String[] args){
    SameClass sp = new SameClass();
    System.out.println(sp.sameVar) // 다른 패키지에 속하는 자식 클래스까지 허용
  }
}
```

#### 기타 제어자


### 출처
[TCP SCHOOL](https://www.tcpschool.com/java/java_modifier_accessModifier)  