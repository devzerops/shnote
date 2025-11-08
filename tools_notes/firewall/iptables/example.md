- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 출력 예제

룰 보기

``` bash
iptables -nL
```
이하 동문
``` bash
view /etc/sysconfig/iptables
```

Rule의 번호값 보기
``` bash
iptables -L --line-numbers
```

## Rule_Set
들어오는 TCP포트 80을 막기
```bash
iptables -A INPUT -p tcp --dport 80 -j DROP 
```

## Rules 저장방법

``` bash
iptables-save > 20221010.rules
```

## 복원예제

``` bash
iptables-restore < 20221010.rules
```

## delete

`iptables -L --line-numbers`으로 확인한 NUMBER RULE 지우기
``` bash
iptables -D INPUT 2
```

## all delete

``` bash
iptables -F
```

## 테스트 환경 구축

방화벽 설정을 확인할 service 간단하게 실행
```bash
sudo apt -y upgrade && \ 
sudo apt -y install apache2 && \
sudo service apache2 start
```

port 보기
``` bash 
nmap localhost
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
