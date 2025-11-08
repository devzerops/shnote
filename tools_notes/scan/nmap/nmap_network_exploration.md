- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
TCP SYN 핑을 이용한 스캐닝
``` bash
nmap -sn -PS 192.168.1.0/24
```

TCP ACK 핑을 이용한 스캐닝
``` bash
nmap -sn -PA 192.168.0.1/24
```

``` bash
nmap -sn -PA22,80,440 google.com
```

``` bash
nmap -sn -PE scanme.nmap.org
```

프록시를 거치는 스캔
``` bash
nmap -sV -Pn -n --prixies http://192.168.2.1:3321 google.com
```

IPv6 스캔
``` bash
nmap -6 google.com
```

## UDP 핑 스캔

``` bash
nmap -sn -PU google.com
```

``` bash
nmap -sn -PU1337,2000 google.com
```

``` bash
nmap -sn -PU1337-2000 google.com
```

``` bash
nmap -sn -PU1,1337-2000 google.com
```

## IP 프로토콜을 이용한 호스트를 찾는 핑 스캔

``` bash
nmap -sn -PO google.com
```

## SCTP INIT 을 이용한 호스트를 찾는 핑 스캔

``` bash
nmap -sn -PY22,80,440 google.com
```

``` bash
nmap -sn -PY22-440 google.com
```

``` bash
nmap -sn -PY21,22-440 google.com
```

## mac을 이용한 스푸핑

``` bash
nmap -sn -PR --spoof-mac "MAC-address" google.com
```

## 브로드캐스트를 이용한 호스트를 찾는 핑 스캔

``` bash
nmap --script broadcast-ping
```

``` bash
nmap --script broadcast-ping --script-args broadcast-ping.num_probes=5
```

``` bash
nmap --script broadcast-ping --script-args broadcast-ping.timeout=10000
```

``` bash
nmap --script broadcast-ping --script-args broadcast-ping.interface=wlan0
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
