- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## FTP 설치

기존 ftp는 보안에 취약해서 vsftp를 사용하는데
vsftp는 very secure ftp이다. 말 그대로 보안에 더 강한 ftp이다.
``` bash
sudo apt install -y ftp
```

## FTP 서버 설치

``` bash
sudo apt install -y sftp
```

ftp가 제대로 설치됐으면 아래 명령어를 치게되면 아래와같은 사용자가 만들어진다.

## FTP 접속 계정 만들기

``` bash
sudo adduser dodo
```
아래와 같음

## SFTP 테스트

## dodo계정

``` bash
mkdir dodo_test && \
cd dodo_test  && \
date > dodo_date 
```

``` bash
sftp dodo@localhost
```

``` sftp
cd sftp
```

``` sftp
get dodo_date
```

``` sftp
exit
```

확인해 보면은 다운로드 되는것을 알 수 있고 
sftp에 접속했던 현재 디렉토리 기준으로 있는 파일을 put으로 보낼수 있다.

``` sftp
put testfile
```

## 다른것으로도 접속

## FTP로 Kaist FTP 서버 접속

``` bash
ftp ftp.kaist.ac.kr
```

id: ftp
passwd: (enter)

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
