# Git Advanced
## 복습

- Git은 파일 관리를 총 5가지로 구분한다.
1. Untracked
    - git이 추적하지 않는 파일
    - 윈도우 기준으로 폴더 내부에 `.git` 폴더가 존재하면 자동으로 추적된다.
    - `.gitignore`를 사용해 폴더나 파일은 Untracked 상태로 만들 수 있다.
2. Tracked
    - git이 변화를 감지하고 있는 상태
    - Tracked 상태는 아래 3가지로 나눌 수 있다.
        - Unmodified
            - 수정하지 않은 파일 ( == 최신 파일)
            - Local Git에 올라간 파일과 비교했을 때, 수정되지 않은 상태
        - Modified (vscode : Changes)
            - 수정한 파일
        - Staged (vscode : Staged Changes)
            - commit을 하기 위해 기다리는 상태
            - 이 영역을 **Staging Area** 라고 부름
- 명령어 정리 : Local Git Repository

```
현재 디렉토리를 Git 작업 디렉토리로 초기화 : git init

Unmodified -> Modified : 파일 수정

Modified -> Staged : git add

Staged -> Local Repository : git commit

현재 Git 파일 상태 확인 : git status

Git 작업 내역 확인 : git log
    - commit id, 작성자, 날짜, commit message 확인 가능
    - 종료 명령어 : q

```

```
1. 최초 작업 시
Local Repo -> Remote Repo : git remote add
Remote Repo -> Local Repo : git clone

2. 진행 중일 때
local repo -> remote repo : git push
remote repo -> local repo : git pull

```

## Git Undoing

1. Working Directory 작업 단계
    - working directory에서 수정한 파일을 수정 전으로 되돌리기
    - git restore를 통해 되돌리면, 해당 내용을 복원할 수 없으니 주의‼
    - `git restore [파일이름]`
2. Staging Area 작업 취소
    - Staged 상태의 파일을 Modified 상태로 되돌림 (add 취소)
    - root-commit 여부에 따라 두가지로 나뉨
        - root-commit : 이전에 commit을 한 적이 있는 파일이면 True
        - root-commit이 없는 경우 : `git rm --cached [파일명]`
        - root-commit이 있는 경우 : `git restore --staged [파일명]`
3. Commit 내용 변경
    - `git commit --amend` 누르면 vim으로 커밋 메시지 수정 가능
4. Repository 작업 취소
    - Commit을 완료한 파일을 Staging Area로 되돌리기 (commit 취소)
    - 명령어 : `git commit --amend`
    - Staging Area에 새로 올라온 내용이 없다면 : **직전 Commit의 메시지만 수정**
    - Staging Area에 새로 올라온 내용이 있다면 : **직전 Commit을 새로운 Commit으로 덮어쓰기**
    - 이 때, Staging Area의 모든 내용이 Commit에 포함된다.
    - Commit 메시지 수정을 위해 Vim 편집기가 열림
    - 입력모드(i) : 문서 편집 가능
    - 명령모드(esc)
        - 명령 모드는 `:` 과 함께 사용한다.
        - 저장 후 종료 (:wq)
        - 강제 종료 (:q!)
            - 리눅스에서 특정 동작을 강제로 수행하게 하는 것 : `!`

## Branch

1. 브랜치 생성
    - `git switch -c [branch name]`
2. 이전 브랜치로 이동
    - `git switch -`
3. 브랜치 삭제
    - 머지된 브랜치만 삭제가능 : `git branch -d [branch name]`
    - force 삭제 : `git branch -D [branch name]`