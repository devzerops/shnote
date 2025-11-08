- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 포트 리스트 사이트
[포트 리스트](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)  

## 현재 설정되어 있는 iptables의 규칙(rule)을 전부 제거
``` bash
iptables -F
```

## 들어오는 모든 패킷을 거부한다
``` bash
iptables -P INPUT DROP
```

## IP주소가 192.168.5.13인 호스트로부터 들어오는 패킷을 허가한다
``` bash
iptables -A INPUT -s 192.168.5.13 -j ACCEPT
```


## 포트번호 20 - 30번까지 차단하고, ssh 포트인 22번만 허가한다 거부 메시지 없이 무조건 거절한다
``` bash
iptables -F INPUT && \
iptables -A INPUT -p tcp --dport 22 -j ACCEPT && \
iptables -A INPUT -p tcp --dport 20:30 -j DROP
```


## rule allow 

1. tcp 22번 인바운드 포트 허용
2. tcp 80번 인바운드 포트 허용
3. tcp 443번 인바운드 포트 허용
4. tcp 8080번 인바운드 포트 허용

``` bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT &&\
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT &&\
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT &&\
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
```

## rule deny

1. FTP TCP 데이터 포트 차단
2. FTP TCP 제어포트 차단
3. dns TCP 포트 차단
4. dns UDP 포트 차단
``` bash
sudo iptables -A INPUT -p tcp --dport 20 -j DROP &&\
sudo iptables -A INPUT -p tcp --dport 21 -j DROP &&\
sudo iptables -A INPUT -p tcp --dport 53 -j DROP &&\
sudo iptables -A INPUT -p udp --dport 53 -j DROP &&\
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
