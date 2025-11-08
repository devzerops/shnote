- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## :norm

여러줄 동시 처리

##문자 맨앞에 추가
```bash
:norm i#
```

앞에 문자가 n개 지워짐
```bash
:norm 1x
:norm 2x 
```

마즈막 문자 n개를 지움

```bash
:norm $1
```

## /

특정 문자만 필터해서 찾음

hello 문자만 필터해서 찾음
```bash
/hello
```

대소문자 구분없이 검색하기

\ 붙히는순간 모든 문자가 대소문자 구분 안함

``` bash
/hello\c
```

## 영구적용

~/.bashrc에 아래와 같은 명령어를 추가해주자

``` bash
set ignorecase
```

## :!

    vim에서 특수 terminal command 수행 가능

```bash
:! sudo apt -y update 
```


## 특정 문자 치환

``` bash
%s/foo/boo
```

대소문자 구분없이
``` bash
%s/foo/boo/i
```

전역으로 치환
``` bash
%s/foo/boo/g
```

바꿀지 물어보기
``` bash
%s/foo/boo/c
```


## 특수사용
줄바꿈 없애기
``` bash
%s/\n//g
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
