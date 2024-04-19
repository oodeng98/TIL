# Git workflow

## Feature Branch Workflow

Shared repository model(저장소의 소유권이 있는 경우)

1. 각 사용자는 원격 저장소의 소유권을 가진 상태이므로 clone을 통해 저장소를 로컬에 복제
2. 기능 추가를 위해 branch 생성 및 기능 구현
3. 기능 구현 후 원격 저장소에 브랜치 반영

```bash
git push origin 브랜치명
```

5. 병합 후 병합 완료된 브랜치 삭제
6. master 브랜치로 switch
7. 병합된 master 내용을 pull
8. 원격 저장소에서 병합 완료 된 로컬 브랜치 삭제
9. 새로운 기능 추가를 위해 브랜치 생성 및 앞선 과정 반복

## Forking Workflow

Fork & Pull model(저장소의 소유권이 없는 경우)

1. 저장소의 소유권이 없으므로 fork를 통해 복제
2. 복제한 저장소를 clone하여 로컬에 복제
3. 추후 로컬 저장소를 원본 원격 저장소와 동기화하기 위해 URL을 연걸

```bash
git remote add upstream [원본URL]
```

4. 기능 추가를 위해 branch 생성 및 기능 구현
5. 기능 구현 후 원격 저장소에 브랜치 반영
6. 원본 저장소에 Pull Request 요청
7. 원본 저장소에서 병합 완료, 복제 저장소에서 병합 완료 된 브랜치 삭제
8. 로컬에서 master 브랜치로 switch 후 병합 완료 된 로컬 브랜치 삭제
9. 원본 저장소에서 병합된 master의 내용을 pull
10. 새로운 기능 추가를 위해 브랜치 생성 및 앞선 과정 반복
