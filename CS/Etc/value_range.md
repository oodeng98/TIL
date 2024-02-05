# 값의 범위

## 의문점
왜 값의 범위는 -128~127과 같이 양수 범위의 절댓값이 1이 작은가?

## 설명
컴퓨터는 0과 1의 이진수만 이해할 수 있기에 음수를 표현하는 방식이 정해져있다.
그래서 부호 절댓값 방식이라는 방법을 사용한다.

### 부호 절댓값 방식(Most Significant Bit, MSB)
최상위 비트로 부호를 표현하고, 나머지 비트로 해당 정수의 **절댓값**을 나타낸다.  
최상위 비트를 부호 비트로 사용하기에 표현할 수 있는 절댓값의 범위는 절반으로 줄어든다.  
최상위 비트가 0이면 양수, 1이면 음수
![MSB 사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FozixR%2Fbtrp0Zon50b%2FenrVSxklJV8ulQWiTlehG0%2Fimg.png)

하지만 연산이 간단하지 않다는 단점 존재
![연산](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FceOzk3%2Fbtrp0Krlscd%2FgHhU2BnFsXhILUXycmO1Wk%2Fimg.png)

### 1의 보수
그래서 연산을 간단하게 해결할 수 있는 1의 보수를 사용  
1의 보수는 해당 양수의 모든 비트를 반전시켜 음수를 표현하는 방법  
부호 절댓값 방식과 동일하게 최상위 비트를 부호 비트로 사용
![1의 보수](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FNP59z%2FbtrpVvWwJPV%2FKLsvPFIOjWp9pvYO8iasK1%2Fimg.png)

그래서 연산을 간단하게 해결할 수 있다.
![1의 보수 연산](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDxVAI%2FbtrpWyyFWha%2F9zTkVMTYvSZYd7K5TXE5hK%2Fimg.png)
### 출처
[hexabrain](https://blog.hexabrain.net/367)