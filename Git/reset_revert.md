# Reset & Revert
공통점
- 과거로 되돌린다  
차이점
- 과거로 되돌리겟다는 내용도 기록되는가?(commit 이력에 남는가?)

## 1. git reset
- 시계를 마치 과거로 돌리는 듯한 행위로써, 특정 커밋 상태로 되돌아감
- 특정 커밋으로 되돌아갔을 때, 해당 커밋 이후로 쌓아 놨던 커밋들은 전부 사라짐
```bash
git reset [옵션] <커밋 ID>
```
옵션
1. `--soft`
- 돌아가려는 커밋으로 되돌아가고, 이후에 commit된 파일들을 staging area로 돌려놓음(commit 하기 전 상태)
- 즉, 다시 커밋할 수 있는 상태가 됨
2. `--mized`
- 돌아가려는 커밋으로 되돌아가고, 이후에 commit 된 파일들을 working directory로 돌려놓음(add 하기 전 상태)
- 즉, unstage 된 상태로 남아있음
- 기본 값
3. `--hard`
- 돌아가려는 커밋으로 되돌아가고, 이후에 commit 된 파일들을 모두 working directory에서 삭제
- 단, Untracked 파일은 그대로 Untracked로 남음

### 옵션별 결과
돌아가려는 커밋 이후에 커밋된 `2.txt`, `3.txt`가 어떻게 처리되는지 확인하기
```bash
# 사전 작업
touch 1.txt
git add .
git commit -m "first"

touch 2.txt
git add .
git commit -m "second"

touch 3.txt
git add .
git commit -m "third"

git log --oneline
# b04aa09 (HEAD -> master) third
# d737018 second
# 860a419 first

# 1. --soft
git reset --soft 860a
git status
# On branch master
# Changes to be committed:
#   (use "git restore --staged <file>..." to unstage)
#         new file:   2.txt
#         new file:   3.txt

# 2. --mixed
git reset 860a
git status
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#         2.txt
#         3.txt

# 3. --hard
git reset --hard 860a
git status
# On branch master
# nothing to commit, working tree clean

```
- 옵션 한눈에 보기
특정 커밋으로 reset했을 때 특정 커밋 이후에 커밋되었던 파일들의 상태  
|옵션|working directory|staging area|repository|
|----|-----------------|------------|----------|
|--soft||V|HEAD가 특정 커밋을 가리킴|
|--mixed|V||HEAD가 특정 커밋을 가리킴|
|--hard|||HEAD가 특정 커밋을 가리킴|

단, --hard 옵션 사용 시 기존의 Untracked 파일은 여전히 Untracked로 남음
<!-- 그림으로 이해하는 git reset부터 정리 -->
## git revert