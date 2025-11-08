- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## hashlimit

``` bash
iptables -I INPUT -d 192.168.0.18 -p tcp --dport 80 -m hashlimit --hashlimit-name flood_list --hashlimit-above 20/second --hashlimit-mode srcip --hashlimit-burst 100 --hashlimit-htable-expire 3000 -j DROP

iptables -I INPUT -d 192.168.0.18 -p tcp --dport 80 -m hashlimit --hashlimit-name flood_list --hashlimit-above 20/second --hashlimit-mode srcip --hashlimit-burst 100 --hashlimit-htable-expire 3600 -j LOG --log-prefix "[Im attack]:"
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
