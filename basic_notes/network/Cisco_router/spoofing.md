- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## spoofing deny
스푸핑 방지 예시
``` bash
router# configure terminal
router(config)# access-list <ACL 번호> deny ip 0.0.0.0 0.255.255.255 any
router(config)# access-list <ACL 번호> deny ip 10.0.0.0 0.255.255.255 any
router(config)# access-list <ACL 번호> deny ip 127.0.0.0 0.255.255.255 any
router(config)# access-list <ACL 번호> deny ip 169.254.0.0 0.0.255.255 any
router(config)# access-list <ACL 번호> deny ip 172.16.0.0 0.15.255.255 any
router(config)# access-list <ACL 번호> deny ip 192.0.2.0 0.0.0.255 any
router(config)# access-list <ACL 번호> deny ip 192.168.0.0 0.0.255.255 any
router(config)# access-list <ACL 번호> deny ip 224.0.0.0 15.255.255.255 any
router(config)# access-list <ACL 번호> permit ip any any
``시

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
