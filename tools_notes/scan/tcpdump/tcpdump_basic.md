- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
인터페이스 eth0 을 보여줌
```bash
tcpdump -i eth0 
```

카운터 10개만 덤프
```bash
tcpdump -i eth0 -c 10 
```

TCP 80 포트로 통신하는 패킷 덤프
```bash
tcpdump -i eth0 tcp port 80 
```

출발지 IP가 192.168.0.1인 패킷 덤프
```bash
tcpdump -i eth0 src 192.168.0.1 
```

목적지 IP가 192.168.0.1인 패킷 덤프
```bash
tcpdump -i eth0 dst 192.168.0.1 
```

목적지 IP가 192.168.0.1이면서 TCP 80 포트인 패킷 보여줌
```bash
tcpdump -i eth0 src 192.168.0.1 and tcp port 80 
```

목적지IP가 192.168.0.1인 패킷 보여줌
```bash
tcpdump -i eth0 dst 192.168.0.1 
```

특정 호스트 IP로 들어오거가 나가는 양방향 패킷 모두 덤프
```bash
tcpdump host 192.168.0.1 
```

특정 호스트 중에서 출발지가 192.168.0.1인 것만 덤프
```bash
tcpdump src 192.168.0.1 
```

특정 호스트 중에서 목적지가 192.168.0.1인 것만 덤프
```bash
tcpdump dst 192.168.0.1 
```

포트 양뱡항으로 3389이면 덤프
```bash
tcpdump port 3389  
```

출발지 포트가 3389인 것만 덤프
```bash
tcpdump src port 3389
```

목적지 포트가 3389인 것만 덤프
```bash
tcpdump dst port 3389 
```

UDP 이고 출발지 포트가 53이면 덤프  
```bash
tcpdump udp and src port 50
```

출발지 IP가 192.168.0.1이고 목적지 포트가 22 가 아닌 패킷 덤프
```bash
tcpdump src 192.168.0.1 and not dst port 22 
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
