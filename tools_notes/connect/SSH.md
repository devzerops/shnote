- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## SSH란?
* Telnet, RSH, Rlogin 등과 같은 취약점을 가진것을 대체하기 위해 만든 도구
* TCP/IP 프로토콜을 사용한다.
* 기본 포트번호: 22번

## 원리
[SecurityNotes_SSH.md](../../security_note/tool_principle/SSH.md)

## 접속방법

``` bash
ssh kali@192.168.35.33
```

``` bash
ssh -o TCPKeepAlive=yes -o ServerAliveCountMax=20 -o ServerAliveInterval=15 kali@192.168.35.95
```
## 호스트 기반 키인증
``` bash
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:U7xoMzYPWhBQmrp4n5ifdl7b+iYSn2Am+cdsp+qfPm8.
Please contact your system administrator.
Add correct host key in C:\\Users\\dbstj/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in C:\\Users\\dbstj/.ssh/known_hosts:33
ECDSA host key for 192.168.35.168 has changed and you have requested strict checking.
Host key verification failed.

```

## 키인증
``` bash
PS C:\Users\dbstj> ssh-keyscan 192.168.35.168 >> ~/.ssh/known_hosts
## 192.168.35.168:22 SSH-2.0-OpenSSH_8.7
## 192.168.35.168:22 SSH-2.0-OpenSSH_8.7
## 192.168.35.168:22 SSH-2.0-OpenSSH_8.7
PS C:\Users\dbstj> ssh server@192.168.35.168
server@192.168.35.168's password:
Web console: https://localhost:9090/ or https://10.60.1.2:9090/

Last login: Sat Mar 25 17:57:57 2023 from 10.20.1.2
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
