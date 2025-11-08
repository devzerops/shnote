- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## VTY 접근 허용 IP 설정
``` bash
Router# config terminal
Router(config)# access-list <ACL 번호> permit <IP 주소> 
Router(config)# access-list <ACL 번호> deny any log 
Router(config)# line vty ?
<0X4> First Line number
Router(config)# line vty 0 4
Router(config)# access-class <ACL 번호> in
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
