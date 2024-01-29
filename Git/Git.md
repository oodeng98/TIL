# Git

## Git이란?
 - 분산 버전 관리 시스템

### Git의 3개 영역

1. Working Directory
    * 현재 작업중인 영역
    * *Local 생각하면 편함*
2. Staging Area
    * 버전 관리를 위한 파일, 폴더를 선별하기 위한 목적
    * *add 후 파일이 잠시 거쳐가는 공간*
3. Repository
    * 버전이 저장되는 공간
    * *Github 생각하면 될 듯*

### Git 기본 명령어

* `git init` : git으로 관리하겠다고 선언
    * 이미 git으로 관리되는 영역 내부 폴더에서 다시 `git init`을 하면 안됨
    * (submodule에 대한 내용으로 응용 등급이니 나중에 확인)
* `git add 파일명or경로` : Working Directory에서 작업한 파일or폴더를 Staging Area로 전달하는 명령어  
    * commit을 찍기 위해(버전을 생성하기 위해 -> 변경 사항을 기록하기 위해) 전달
* `git commit -m 'commit message'` : 실제 버전을 생성하는 명령어
    * [좋은 commit message 작성법](./commit_message_rule.md)
    아래 내용은 경령누나 내용을 가져온 것으로, 나에게 맞게 전부 수정할 것
* `git status` : 현재 git의 상태를 확인할 수 있는 명령어
    * untracked : 아직 관리된 적 없는 파일을 의미
    * modified : 관리되고 있는 파일이 수정된 경우
        * 붉은색: working directory에서 파일이 변경되거나 생성된 경우
        * 녹색: staging area에 위치한 파일인 경우
* `git log` : 현재 저장된 commit 히스코리를 알 수 있다.
    * `--oneline`
* `git config --global -l` : : 현재 git 설정 정보를 알려주는 명령어


# Git 원격 저장소

* github : git을 이용한 원격 저장소를 제공하는 서비스

## 원격 저장소에 push하기!

1. 원격 저장소를 생성한다.
2. 생성한 저장소를 로컬에 등록한다.
    * `git remote add 별칭 원격저장소주소`
3. git push 별칭 브랜치명 : 원격 저장소에 버전 정보를 업로드한다.

* 로컬 저장소는 원격 저장소와 자동 동기화 되지 않는다(어리없는 소리~).
    * 항상 pull 하는 것을 생활화 하자
    * 추천 pull - 작업 - add - commit - push

* pull vs. clone
    * pull: git 저장소가 로컬에 이미 있을 때
    * clone : git 저장소가 로컬에 없을 때
        * clone을 하면 원격 저장소가 그대로 로컬에 복제된다. (.git 폴더와 함께)
## gitignore 란?
* 버전 관리를 하지 않을 파일이나 폴더 및 경로를 등록하는 파일
    * 한 번이라도 등록되지 않은 파일, 폴더, 경로 명만 사용해야 한다.
    * 그래서 git init 과 같이 .gitignore 파일 생성이 권장 된다. (그래야 실수를 덜 함)
* 만약 관리되고 있는 파일을 gitignore에 등록하려면
    * 관리되고 있느 파일을 버전 관리 목록에서 제외시켜야 함
        * `git rm --cahced 파일 or 폴더`

* gitignore를 쉽게 작성하는 방법
    * [gitignore.io](https://www.toptal.com/developers/gitignore/) 에서 설정하면 꿀!
    * 설정 목록은 사용언어, 환경, 에디터, 프레임워크
        > ex: Python, VisualStudioCode, Django, Jupyternotebook, pycharm, vue, node
    * 생성된 내용을 그대로 복사하여 .gitignore에 붙여넣기