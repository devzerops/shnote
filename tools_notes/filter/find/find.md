- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## find 명령어

### find [-H] [-L] [-P] [-D debugopts] [-Olevel] [starting-point...] [expression]

## 옵션

* P : 심볼릭 링크를 따라가지 않고, 심볼릭 링크 자체 정보 사용.
* L : 심볼릭 링크에 연결된 파일 정보 사용.
* H : 심볼릭 링크를 따라가지 않으나, Command Line Argument를 처리할 땐 예외.
* D : 디버그 메시지 출력.

## 표현식 

* name : 해당 이름의 파일을 찾음. 해당 이름에는 정규 표현식을 활용할 수 있음
* type : 지정된 파일 타입에 해당하는 파일 검색
* user : 해당 유저에게 속한 파일 검색
* empty : 빈 디렉토리 혹은 크기가 0인 파일 검색
* delete : 검색된 파일 혹은 디렉토리 삭제
* exec : 검색된 파일에 대해 지정된 명령 실행
* path : 지정된 문자열 패턴에 해당하는 경로에서 검색.
* print : 검색 결과를 출력. 검색 항목은 newline으로 구분. (기본 값)
* print0 : 검색 결과를 출력. 검색 항목은 null로 구분.
* size : 파일 크기를 사용하여 파일 검색.
* mindepth : 검색을 시작할 하위 디렉토리 최소 깊이 지정.
* maxdepth : 검색할 하위 디렉토리의 최대 깊이 지정.
* atime : n일 이내에 액세스된 파일을 찾음.
* ctime : n일 이내에 파일 속성이 변경된 시간 파일을 찾음.
* mtime : n일 이내에 수정되거나 만들어진 파일을 찾음.
* cnewer file : 해당 파일보다 최근에 수정된 파일을 찾음.
* find 명령어는 다른 명령어와는 달리 옵션은 거의 사용되지 않고 이


## 예제
|      현재 디렉토리에 있는 파일 및 디렉토리 리스트 표시		|                    find						|
|:-----------------------------------------------------------:	|:------------------------------------------:	|
|      대상 디렉토리에 있는 파일 및 디렉토리 리스트 표시		|                 find [PATH]					|
| 현재 디렉토리 아래 모든 파일 및 하위 디렉토리에서 파일 검색	|             find . -name [FILE]				|
|           전체 시스템(루트 디렉토리)에서 파일 검색			|             find / -name [FILE]				|
|         파일 이름이 특정 문자열로 시작하는 파일 검색			|             find . -name "STR*"				|
|          파일 이름에 특정 문자열이 포함된 파일 검색			|            find . -name "*STR*"				|
|          파일 이름이 특정 문자열로 끝나는 파일 검색			|             find . -name "*STR"				|
|            빈 디렉토리 또는 크기가 0인 파일 검색				|                find . -empty					|
|          특정 확장자를 가진 모든 파일 검색 후 삭제			|        find . -name "*.EXT" -delete			|
|      검색된 파일 리스트를 줄 바꿈 없이 이어서 출력하기		|         find . -name [FILE] -print0			|
|                파일 또는 디렉토리만 검색하기					|         find . -name [FILE] -type f			|
|                파일 크기를 사용하여 파일 검색				|     find . -size +[N]c -and -size -[M]c		|
|        검색된 파일에 대한 상세 정보 출력. (find + ls)			|    find . -name [FILE] -exec ls -l {} \;		|
|           검색된 파일의 라인 수 출력. (find + wc)				|    find . -name [FILE] -exec wc-l {} \;		|
|          검색된 파일에서 문자열 찾기. (find + grep)			| find . -name [FILE] -exec grep "STR" {} \;	|
|         검색 결과를 파일로 저장. (find, redirection)			|      find . -name [FILE] > [SAVE_FILE]		|
|    검색 중 에러 메시지 출력하지 않기 (find, redirection)		|      find . -name [FILE] 2> /dev/null			|
|                 하위 디렉토리 검색하지 않기					|       find . -maxdepth 1 -name [FILE]			|
|                검색된 파일 복사. (find + cp)					|  find . -name [FILE] -exec cp {} [PATH] \;	|


## 시간 속성

``` bash

server@node3:~$ sudo find /home/ -mtime +7 -type f
[sudo] password for server:
/home/server/.python_history
/home/server/capture.pcap
/home/server/echo_mpclient
/home/server/.bash_logout
/home/server/.vimrc
/home/server/.ssh/authorized_keys
/home/server/.ssh/known_hosts
/home/server/.ansible/tmp/ansible-tmp-1674058767.5981522-369755-281458500853804/AnsiballZ_ufw.py
/home/server/.bashrc
/home/server/.profile
/home/server/token.txt
/home/dodo/.bash_history
/home/dodo/.bash_logout
/home/dodo/.ssh/known_hosts
/home/dodo/.bashrc
/home/dodo/.profile
`` `

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
