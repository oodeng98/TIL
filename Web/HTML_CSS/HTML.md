# HTML

## WWW(World Wide Web)

인터넷으로 연결된 컴퓨터들이 정보를 공유하는 거대한 정보 공간

Web

- Web site, Web application 등을 통해 사용자들이 정보를 검색하고 상호 작용하는 기술

Web site

- 인터넷에서 여러 개의 Web page가 모인 것으로, 사용자들에게 정보나 서비스를 제공하는 공간

## Web page

HTML, CSS등의 웹 기술을 이용하여 만들어진 Web site를 구성하는 하나의 요소

Web page의 구성 요소

- HTML: Structure
- CSS: Styling
- Javascript: Behavior

## HTML

HyperText Markup Language  
웹 페이지의 의미와 구조를 정의하는 언어

HyperText

- 웹 페이지를 다른 페이지로 연결하는 링크
- 참조를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트

Markup Language

- 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어

### HTML 구조

```html
<!DOCTYPE html>
```

- 해당 문서가 html로 문서라는 것을 나타냄

```html
<html></html>
```

- 전체 페이지의 콘텐츠를 포함

```html
<title></title>
```

- 브라우저 탭 및 즐겨찾기 시 표시되는 제목으로 사용

```html
<head></head>
```

- html문서에 관련된 설명, 설정 등으로 사용자에겐 보이지 않음

```html
<body></body>
```

- 페이지에 표시되는 모든 콘텐츠

```html
<p></p>
```

- Paragraph를 의미

```html
<a></a>
```

- Anchor를 의미

```html
<br />
```

- 줄넘김을 의미

```html
<hr />
```

- 선을 그어줌

### HTML Element(요소)

```html
<p>My cat is very grumpy</p>
tag content tag E L E M E N T
```

하나의 요소는 여는 태그와 닫는 태그, 그 안의 내용으로 구성됨  
닫는 태그는 태그 이름 앞에 /가 포함되며 닫는 태그가 없는 태그도 존재

- 태그 사이에는 컨텐츠가 존재 -> 닫는 태그가 없다 -> 컨텐츠가 없다

### HTML Attributes(속성)

```html
<p class="editor-note">My cat is very grumpy</p>
A t t r i b u t e
```

규칙

- 속성은 요소 이름과 속성 사이에 공백이 있어야 함
- 하나 이상의 속성들이 있는 경우엔 속성 사이에 공백으로 구분
- 속성 값은 열고 닫는 따옴표로 감싸야 함
  목적
- 나타내고 싶지 않지만 추가적인 기능, 내용을 담고 싶을 때 사용
- CSS에서 해당 요소를 선택하기 위한 값으로 활용

### HTML Attributes(속성)

HTML의 주요 목적 중 하나는 텍스트 구조와 의미를 제공하는 것

대표적인 HTML Text structure

- Heading & Paragraphs
  - h1~6, p
- Lists
  - ol, ul, li
- Emphasis & Importance
  - em, strong

### HTML 관련 사항

- 요소(태그) 이름은 대소문자를 구분하지 않지만 소문자 사용을 권장
- 속성의 따옴표는 작은 따옴표와 큰 따옴표를 구분하진 않지만 큰 따옴포 권장
- HTML은 프로그래밍 언어와 달리 에러를 반환하지 않기 때문에 작성시 주의

## 참고할만한 사이트

[MDN](https://developer.mozilla.org/ko/)
