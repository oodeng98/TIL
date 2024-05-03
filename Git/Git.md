# Git

## Git이란?

- 분산 버전 관리 시스템

## Git의 3개 영역

1. Working Directory
   - 현재 작업중인 영역
   - _Local 생각하면 편함_
2. Staging Area
   - 버전 관리를 위한 파일, 폴더를 선별하기 위한 목적
   - _add 후 파일이 잠시 거쳐가는 공간_
3. Repository
   - 버전이 저장되는 공간
   - _Github 생각하면 될 듯_

## Git 기본 명령어

- `git init` : git으로 관리하겠다고 선언
  - 이미 git으로 관리되는 영역 내부 폴더에서 다시 `git init`을 하면 안됨
  - (submodule에 대한 내용으로 응용 등급이니 나중에 확인)
- `git add 파일명or경로` : Working Directory에서 작업한 파일or폴더를 Staging Area로 전달하는 명령어
  - commit을 찍기 위해(버전을 생성하기 위해 -> _변경 사항을 기록하기 위해_) 전달
- `git commit -m 'commit message'` : 실제 버전을 생성하는 명령어
  - [좋은 commit message 작성법](./commit_message_rule.md)
- `git status` : 현재 git의 상태를 확인하는 명령어
  - untracked : 아직 관리된 적 없는 파일
  - modified : 관리되고 있는 파일이 수정된 경우
    - 붉은색: working directory에서 파일이 변경or생성된 경우
    - 녹색: staging area에 위치한 파일인 경우
- `git log` : commit 히스토리 보여줌
  - `--oneline`: 각각의 commit log가 한 줄로 깔끔하게
- `git config --global -l` : 현재 git 설정 정보를 알려주는 명령어

## Git 원격 저장소

- github
- gitlab
- yobi
- _etc..._

## 원격 저장소에 Push

1. 원격 저장소를 생성
2. 생성한 저장소를 로컬과 연결
   - `git remote add 별칭 원격저장소주소`
3. git push 별칭 브랜치명: 원격 저장소에 버전 정보를 업로드

- 로컬 저장소는 원격 저장소와 자동 동기화 되지 않음(~~사실 당연한 소리~~)

  - 항상 pull 하는 것을 생활화
  - 추천 순서: pull - 작업 - add - commit - push

- pull vs clone
  - pull: git 저장소가 로컬에 이미 있을 때 작업 상황 동기화
  - clone: git 저장소가 로컬에 없을 때 새로 생성
    - 원격 저장소가 로컬에 복제(.git 폴더도 같이)

### 403 에러 해결 방법

- 403 에러는 해당 레포지토리 주소에 접근 권한이 없을 경우 발생

```bash
remote: Permission to oodeng98/TIL.git denied to ???????.
fatal: unable to access 'https://github.com/oodeng98/TIL.git/': The requested URL returned error: 403
```

1. 작성자(Author) 정보 삭제

```bash
# 기존 Author 정보 확인
git config --global --list
# 기존 Author 정보 있을 시, 초기화
git config --unset --global user.email
git config --unset --global user.name
# 새로운 Author 정보 등록
git config --global user.email '내 이메일'
git config --global user.name '내 이름'
```

2. 로그인 계정 삭제

- 제어판 -> 사용자 계정 -> 자격 증명 관리자 -> Windows 자격 증명 -> github 관련 자격 증명 제거

3. Git push 실행
   ![Git push 실행시 화면](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcvwpoc%2FbtqDvgxGy4Y%2FhNJg8JSP395eFq7eSTCMDk%2Fimg.png)

- 로그인하면 끝

#### [출처](https://somjang.tistory.com/entry/Git-Git-Bash-%ED%84%B0%EB%AF%B8%EB%84%90-%EA%B3%84%EC%A0%95-%EB%B3%80%EA%B2%BD-%EB%B0%A9%EB%B2%95)

## .gitignore란?

- 버전 관리를 하지 않을 파일or폴더or경로를 등록하는 파일
  - 한 번도 등록되지 않은 파일, 폴더, 경로만 사용해야 함
  - git init과 같이 .gitignore 파일 생성을 권장(_실수 덜 하려고_)
- 만약 관리되고 있는 파일을 gitignore에 등록하려면?

  - `git rm --cached 파일or폴더or경로`
  - 관리되고 있는 파일을 버전 관리 목록에서 제외하는 명령어

- gitignore를 쉽게 작성하는 방법
  - [gitignore.io](https://www.toptal.com/developers/gitignore/) 에서 설정
  - 설정 목록은 사용언어, 환경, 에디터, 프레임워크
    > Ex) Python, VisualStudioCode, Django, Jupyternotebook, Pycharm, Vue, Node
  - 생성된 내용을 그대로 복사하여 .gitignore로 사용
