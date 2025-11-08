- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
``` bash
iptables -I INPUT -p tcp -d 192.168.0.18 -m geoip --src-cc US -j DROP && \
iptables -I INPUT -p tcp -s 1.1.1.1 -d 192.168.0.18 -j ACCEPT
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
