# Undoing Things
되돌리기  

## 1. 파일 내용을 수정 전으로 되돌리기
Unmodifying a Modified File
- Modefied 파일 되돌리기  

Working Directory에서 파일을 수정했다고 가정했을 때, 만약 이 파일의 수정 사항을 취소하고, 원래 모습대로 돌리려면 어떻게 해야 하는가?

git restore
- git restore <파일 이름>의 형식을 사용
- git의 추적이 되고 있는, 즉 버전 관리가 되고 있는 파일만 되돌리기 가능
1. 이미 버전 관리가 되고 있는 test.md 파일을 변경 후 저장
```
# test.md

Hello
World <- "World"라는 새로운 내용 추가
-------------------------------------
이후 저장
```
2. git restore를 통해 수정 전으로 되돌림
```bash
git restore test.md
```
```
# test.md

Hello
-------------------------------------
World가 삭제되면서, 수정 전으로 되돌아감
```
- 원래 파일을 덮어썼기 때문에 수정한 내용은 전부 사라짐
- 한번 git restore를 통해 수정을 취소하면, 해당 내용을 복원할 수 없음  

참고
- Git(2.23.0 이전)에서는 아래 명령어를 사용
```bash
git checkout -- <파일 이름>
```

## 2. 파일 상태를 Unstage로 되돌리기
Unstaging a Staged File
- Staging Area와 Working Directory 사이를 넘나드는 방법

git add를 통해서 파일을 Staging Area에 올렸다고 가정할 때, 만약 이 파일을 Unstage 상태로 내리려면 어떻게 해야 하는가?

### 1. git rm --cached  
1. 새 폴더에서 git 초기화 후 진행  
```bash
touch test.md
git add test.md
git status
```
2. Staging Area 에 올라간 test.md 다시 내리기
```bash
git rm --cached test.md

git status
```
### 2. git restore --staged
사전 준비
```bash
git add .
git commit -m "first commit"
```
1. test.md의 내용을 변경하고 git add를 진행
```bash
git add test.md
git status
```
2. Staging Area에 올라간 test.md를 다시 내리기(unstage)
```bash
git restore --staged test.md
git status
```

### Unstage로 되돌리는 명령어가 다른 이유?
1. git rm --cached <file>
- 기존에 커밋이 없는 경우
- to unstage and remove paths only for the staging area
2. git restore --staged <file>
- 기존에 커밋이 존재하는 경우
- the contents are restored from HEAD  

참고
- Git(2.23.0 이전)에서는 아래와 같은 명령어 사용
```bash
git reset HEAD <파일 이름>
```

## 바로 직전 완료한 커밋 수정하기
만약 A라는 기능을 완성하고 "A 기능 완성"이라는 커밋을 남겼다고 가정  
그런데 A기능에 필요한 파일 1개를 빼놓고 커밋했다는 것을 뒤늦게 깨달았다면?  
직전 커밋을 취소하고, 모든 파일을 포함해서 다시 커밋하려면 어떻게 해야 하는가?

### 1. git commit --amend
- 해당 명령어는 2가지 기능을 가지며 상황별로 동작이 다름  
1-1. 커밋 메세지만 수정하기  
- 마지막으로 커밋하고 나서 수정한 것이 없을 때(커밋하자마자 바로 명령어를 실행하는 경우)
```bash
# A기능을 완성하고 커밋
git commit -m "A feature completed"
# 현재 커밋 해시 값 확인해두기
git log
# 커밋 메시지 수정을 위해 다음과 같이 입력
git commit --amend
# Vim 편집기가 열리면서 직전 커밋 메시지를 수정할 수 있음
# 커밋 메시지를 수정하고 저장하면, 새로운 메시지로 변경되며 커밋 해시 값 또한 변경됨
git log
```
1-2. 커밋 재작성
- 실수로 bar.txt를 빼고 커밋해버린 상황
```bash
# 사전 작업
touch foo.txt bar.txt
git add foo.txt
git status

git commit -m "foo & bar"
git status

# 누락된 파일을 staging area로 이동시킴
git add bar.txt
git status
git commit --amend

# Vim 편집기 열림(커밋 메시지 수정 가능)
# Vim 편집기 저장 후 종료하면 직전 커밋이 덮어씌워짐
# 커밋이 새로 추가된 것이 아니며, 마찬가지로 커밋 해시 값 또한 변경됨
git log -p
```

장점
- --amend 옵션으로 커밋을 고치는 작업이 주는 장점은 마지막 커밋 작업에서 뭔가 빠뜨린 것을 넣거나 변경하는 것을 새 커밋으로 분리하지 않고 하나의 커밋에서 처리하는 것
- 예를 들어 '빠진 파일 추가', 등의 커밋을 만들지 않겠다는 말  

중요
- 이렇게 --amend 옵션으로 커밋을 고치는 작업은 추가로 작업한 일이 작다고 하더라도 이전의 커밋을 완전히 새로 고쳐서 새 커밋으로 변경하는 것을 의미
- 이전의 커밋은 일어나지 않은 일이 되는 것이고, 당연히 히스토리에도 남지 않음
<!-- 
2. 이전 커밋 덮어쓰기
- Staging Area에 새로 올라온 내용이 있을 때
근데 이 내용은 어디갔지?
 -->