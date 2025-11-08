- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## SNMP service check

``` bash
Router# show running-config
Router# show snmp
```

## SNMP 켜기 

## SNMP 끄기
``` bash
Router# config terminal
Router(config)# no snmp-server
```

## SNMP ACL 설정
``` bash
Router# config terminal
Router(config)# access-list <ACL 번호> permit <IP 주소>
Router(config)# access-list <ACL 번호> deny any log 
Router(config)# snmp-server community <커뮤니티 스트링> RO <ACL 번호>
```

## SNP 커뮤니티 권한

``` bash
Router# config terminal
Router(config)# snmp-server community <스트링명> RO
Router(config)# snmp-server community <스트링명> RW
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
