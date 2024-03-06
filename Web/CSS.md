# CSS
Cascading Style Sheet  
웹 페이지의 디자인과 레이아웃을 구성하는 언어

## CSS 구문
![CSS구문](/Web/img/CSS.PNG)

## CSS 적용 방법
1. Inline style
2. Internal style sheet
3. External style sheet
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    h2 {
        color: red;
    }
  </style>
  <link rel="stylesheet" href="style.css">
</head>

<body>
  <h1 style="color: blue; background-color: yellow;">Inline Style</h1>
  <h2>Internal Style</h2>
  <h3>External Style</h3>
</body>

</html>
```

## CSS Selector
HTML 요소를 선택하여 스타일을 적용할 수 있도록 하는 선택자

CSS Selector의 종류
- 기본 선택자
  - 전체 선택자(*)
    - HTML 모든 요소를 선택
  - 요소 선택자
    - 지정한 모든 태그를 선택
  - 클래스 선택자
    - 주어진 클래스 속성을 가진 모든 요소를 선택
  - 아이디 선택자
    - 주어진 아이디 속성을 가진 요소를 선택
    - 문서에는 주어진 아이디를 가진 요소가 하나만 있어야 함
  - 속성 선택자 등
- 결합자
  - 자손 결합자(" "(space))
    - 첫번째 요소의 자손 요소들 선택
    ```html
    p span은 <p>안에 있는 모든 <span>을 선택(하위 레벨 상관 없이)
    ```
  - 자식 결합자
    - 첫번째 요소의 직계 자식만 선택
    ```html
    ul > li는 <ul>안에 있는 모든 <li>를 선택(한단계 아래 자식들만)
    ```
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
        color: red;
    }
    h2 {
        color: orange;
    }

    h3, 
    h4 {
        color: blue;
    }
    /* 클래스 선택자 */
    .green {
        /* .이 html의 클래스 속성을 의미 */
        color: green;
    }
    /* id 선택자 */
    #purple {
        color: purple;
    }
    /* 자식 결합자 */
    .green > span {
        font-size: 50px;
    }
    /* 자손 결합자 */
    .green li {
        color: brown;
    }
  </style>
</head>

<body>
  <h1 class="green">Heading</h1>
  <h2>선택자</h2>
  <h3>연습</h3>
  <h4>반가워요</h4>
  <p>과목 목록</p>
  <ul class="green">
    <li>파이썬</li>
    <li>알고리즘</li>
    <li>웹
      <ol>
        <li>HTML</li>
        <li>CSS</li>
        <li>PYTHON</li>
      </ol>
    </li>
  </ul>
  <p class="green">Lorem, <span>ipsum</span> dolor.</p>
</body>

</html>
```

## Specificity
명시도  
결과적으로 요소에 적용할 CSS 선언을 결정하기 위한 알고리즘  
CSS Selector에 가중치를 계산하여 어떤 스타일을 적용할지 결정  
동일한 요소를 가리키는 2개 이상의 CSS 규칙이 있는 경우 가장 높은 명시도를 가진 Selector의 스타일 적용
만약 한 요소에 동일한 가중치를 가진 선택자가 적용될 때 CSS에서 마지막에 나오는 선언이 적용  

- 명시도가 높은 순서
1. Importance
  - !important
2. Inline 스타일
3. 선택자
  - id 선택자 > class 선택자 > 요소 선택자
4. 소스 코드 선언 순서

### 명시도 관련 문서
[그림으로 보는 명시도](https://specifishity.com/)
[명시도 계산기](https://specificity.keegan.st)

## 상속
기본적으로 CSS는 상속을 통해 부모 요소의 속성을 자식에게 상속해 재사용성을 높임

- 상속되는 속성
  - Text 관련 요소(font, color, text-align), opacity, visibility 등
- 상속되지 않는 속성
  - Box model 관련 요소(width, height, border, box-sizing ...)
  - position 관련 요소(position, top/right/bottom/left, z-index) 등
CSS 상속 여부는 MDN 문서에서도 확인할 수 있음
- MDN 각 속성별 문서 하단에서 상속 여부를 확인할 수 있음

## CSS 관련 사항
- CSS inline 스타일은 사용하지 말 것
  - CSS와 HTML 구조 정보가 혼합되어 작성되기 때문에 코드를 이해하기 어렵게 만듦
- CSS의 모든 속성을 외우는 것이 아님
  - 자주 사용되는 속성은 그리 많지 않으며 주로 활용하는 속성 위주로 사용하다 보면 자연스럽게 숙달
  - 그 외의 속성들은 개발하며 필요할 때 마다 검색하여 학습 후 사용
- 속성은 되도록 class만 사용할 것
  - id, 요소 선택자 등 여러 선택자들과 함께 사용할 경우 우선순위 규칙에 따라 예기치 못한 스타일 규칙이 적용되어 전반적인 유지보수가 어려워짐
  - 문서에서 단 한번 유일하게 적용될 스타일의 경우에만 id 선택자 사용을 고려

## CSS 연습 사이트
[CSS Dinner](https://flukeout.github.io/)