# 좋은 Commit Message 작성 방법

## Commit Message 기본 구조
```
<type>(<scope>): <subject>
<blank line>
<body>
<blank line>
(<footer>)
```

## Commit Message 기본 규칙
- Scope

선택사항이며, 변경된 부분을 직접적으로 표기  
Ex) 함수가 변경되었으면 함수명, 메소드가 추가되었으면 클래스명 기입 등

- Subject

첫 글자는 대문자로 입력  
마지막에 .(period)을 찍지 않음
영문 기준 최대 50자  
제목은 명령문의 형태로 작성(동사원형 사용)

- Body

각 줄은 최대 72자  
어떻게 변경했는지보다 무엇을 변경했고 왜 변경했는지를 설명

- Footer

선택사항이며, 관련된 이슈를 언급 Ex) Fixes: #1, #2  
주로 Closes(종료), Fixes(수정), Resolves(해결), Ref(참고), Related to(관련) 키워드를 사용

### Type

|Type 키워드|사용 시점|
|------|---|
|feat|새로운 기능 추가|
|fix|버그 수정|
|docs|문서 수정|
|style|코드 스타일 변경 (코드 포매팅, 세미콜론 누락 등) 기능 수정이 없는 경우|
|design|사용자 UI 디자인 변경 (CSS 등)|
|test|테스트 코드, 리팩토링 테스트 코드 추가|
|refactor|코드 리팩토링|
|build|빌드 파일 수정|
|ci|CI 설정 파일 수정|
|perf|성능 개선|
|chore|빌드 업무 수정, 패키지 매니저 수정 (gitignore 수정 등)|
|rename|파일 혹은 폴더명을 수정만 한 경우|
|remove|파일을 삭제만 한 경우|

### 이슈 관련

|사용 시점|키워드|
|------|---|
|해결|Closes(종료), Fixes(수정), Resolves(해결)|
|참고|Ref(참고), Related to(관련), See also(참고)|

### 예시
```
feat: 회원 가입 기능 구현

SMS, 이메일 중복확인 API 개발

Resolves: #123
Ref: #456
Related to: #48, #45
```

### Commit Message에 활용되는 Emogi(gitmoji)

|Emoji|Description|
|---|---|
|🎨|코드의 형식 / 구조를 개선 할 때|
|📰|새 파일을 만들 때|
|📝|사소한 코드 또는 언어를 변경할 때|
|🐎|성능을 향상시킬 때|
|📚|문서를 쓸 때|
|🐛|버그 reporting할 때, @FIXME 주석 태그 삽입|
|🚑|버그를 고칠 때|
|🐧|리눅스에서 무언가를 고칠 때|
|🍎|Mac OS에서 무언가를 고칠 때|
|🏁|Windows에서 무언가를 고칠 때|
|🔥|코드 또는 파일 제거할 때 , @CHANGED주석 태그와 함께|
|🚜|파일 구조를 변경할 때 . 🎨과 함께 사용|
|🔨|코드를 리팩토링 할 때|
|☔️|테스트를 추가 할 때|
|🔬|코드 범위를 추가 할 때|
|💚|CI 빌드를 고칠 때|
|🔒|보안을 다룰 때|
|⬆️|종속성을 업그레이드 할 때|
|⬇️|종속성을 다운 그레이드 할 때|
|⏩|이전 버전 / 지점에서 기능을 전달할 때|
|⏪|최신 버전 / 지점에서 기능을 백 포트 할 때|
|👕|linter / strict / deprecation 경고를 제거 할 때|
|💄|UI / style 개선시|
|♿️|접근성을 향상시킬 때|
|🚧|WIP (진행중인 작업)에 커밋, @REVIEW주석 태그와 함께 사용|
|💎|New Release|
|🔖|버전 태그|
|🎉|Initial Commit|
|🔈|로깅을 추가 할 때|
|🔇|로깅을 줄일 때|
|✨|새로운 기능을 소개 할 때|
|⚡️|도입 할 때 이전 버전과 호환되지 않는 특징, @CHANGED주석 태그 사용|
|💡|새로운 아이디어, @IDEA주석 태그|
|🚀|배포 / 개발 작업 과 관련된 모든 것|
|🐘|PostgreSQL 데이터베이스 별 (마이그레이션, 스크립트, 확장 등)|
|🐬|MySQL 데이터베이스 특정 (마이그레이션, 스크립트, 확장 등)|
|🍃|MongoDB 데이터베이스 특정 (마이그레이션, 스크립트, 확장 등)|
|🏦|일반 데이터베이스 별 (마이그레이션, 스크립트, 확장명 등)|
|🐳|도커 구성|
|🤝|파일을 병합 할 때|

### 출처
[aeiou Repository](https://jane-aeiou.tistory.com/m/93)  
[lucid_dream:티스토리](https://nohack.tistory.com/17)  
[archivvonjang.log](https://velog.io/@archivvonjang/Git-Commit-Message-Convention)  