- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
git command 
============


 

> git add command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git add file | git add \* | 현 위치에 있는  파일을 커밋준비 상태로 기록 |
| git add (file) | git add hello.txt | 특정 파일을 git에 임시저장 하는 명령어 이다. |


> git checkout command

| git checkout (branch\_name) | git checkout my\_branch | 특정 브랜치로 이동 |


> git reset command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git reset HEAD (file\_name) | git reset HEAD hello.txt | git add로 추가된 파일의 특정 파일 삭제 |


> git commit command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git commit --amend "" | git commit --ament "hello" | git commit -m 으로 잘못입력한 메시지 재 입력 명령어 |
| git commit -m "(message)" | git commit -m "테스트요~" | add 로 기록한 파일을 pull 가능하게 저장 |

> git rebase command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git rebase -i HEAD~n | git commit -i HEAD~3 | commit을 n개 불러온다 옵션을 보고 고르면 된다 |

> git show command

| git show | git show | 커밋한 정보 보기 |

> git reset command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git reset HEAD^ | git reset HEAD^ | 가장 최근의 커밋한 정보를 지움 |
| git reset --(option*) (commit id) | git reset --soft  9f7c0e4144575a3327e56b442a0f6d3b|  option에 따라 commit된 파일을 돌려놓거나 삭제함 |
| git reset HEAD~n | git reset HEAD~1   | 현재로부터 과거커밋까지 n개만큼 삭제 |


> git branch command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git branch (branch\_name) | git branch my\_branch | 특정 브랜치 만들기 |
| git branch -d (branch\_name) | git branch -d my\_branch | 특정 브랜치 삭제 |
| git branch -m (olg) (new) | git branch -m my\_old my\_new | 브랜치의 이름 재 지정 |
| git diff --name-status (branch) | git diff --name-status main | 현재 브랜치와 다른 브랜치 비교 |
| git checkout (branch) (file or dir) | git checkout main test/hello.txt | 다른 브랜치의 파일을 가져오는 명령어 |
| git checkout (branch\_name) | git checkout my\_branch | 특정 브랜치로 이동 |

> git remote commad

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git remote add (remote\_name) (url) | git remote add hello   [https://github.com/bakapopcon/bakapopcon.github.io](https://github.com/bakapopcon/bakapopcon.github.io) | 원격 저장소에 저장하는 명령어이다. |
| git remote rm (remote) | git remote rm origin | 저장소를 하나 삭제한다. |

> git status command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git status | git status | add로 추가된 git의 파일 상태 확인 명령어 |


> git branch command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git diff --name-status (branch) | git diff --name-status main | 현재 브랜치와 다른 브랜치 비교 |
| git checkout (branch) (file or dir) | git checkout main test/hello.txt | 다른 브랜치의 파일을 가져오는 명령어 |
| git diff --staged  | git diff --staged | 현재와 이전에 add 를 비교 |

> git clone command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git clone -b (branch) --single-branch (url) | git clone -b next --single-branch (url) |  특정 브랜치의를 가져오고 다른 브랜치   history은 안가져옴 |
| git clone -b (branch) (url) | git clone -b next  (url) | 위와 비슷하지만 차이점은   branch의 history도 가져옴 |
| git clone --depth n | git clone --depth 1 | 원격 저장소로부터 가장 마즈막 커밋만 가져옴 |


> git log command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git log | git log | 모든 이력 보기 |
| git log -p | git log -p | 변경 사항과 패치를 보여줌|
| git log -n | git log -3 | n개만큼의 항목만 보이도록 해줌|

> git rm command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
|git rm (file) | git rm README.md |원격 저장소와 로컬 저장소에 파일 삭제 |
|git rm --cached (file) | git rm --cached README.md | 원격저장소의 파일 삭제 |

> git push command

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git push (remote 주소) (branch\_name) | git push hello master | 저장소의 주소로 가장 최근에 커밋한   branch를 저장소로 파일 업로드를 하는것 |

> git config 관련 명령어

| 명령어 | 예시 | 설명 |
| --- | --- | --- |
| git config --global user.() "" | git config --global user.name "me"   git config --global user.email "me@gmail.com" | 내 환경에서의 설정을 입력하는거다.   각 git마다 다르게 하고싶음 --local로 바꾸자 |

## option list


> git reset option command

|option* | 뜻 |
| --- | --- |
|soft | commit된 파일을 이전으로 돌려놓음 (staging area) |
|mixed | commit된 파일을 add 이전으로 돌려놓음 (working dir)|
|hard | commit된 파일중 tracked 파일들 working dir에서 삭제 |

> git config options command

| 커멘드|사용법|
| --- | --- |
|--global| 전역단 설정|
|--local| 지역단 설정|
|--system|시스템단 설정|
|--list| 정보조회|

| 커멘드|사용법|
| --- | --- |
|user.name| 사용자 이름 변경|
|user.email| 사용자 이메일 변경|
|color.ui|git의 출력결과 색상 활성화(보통 auto 설정)|

----

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
