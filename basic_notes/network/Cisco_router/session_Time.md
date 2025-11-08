- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 점검 방법
Cisco IOS

``` bash
Router# show running-config
```

## Session time 설정

## Console
``` bash
Router# config terminal
Router(config)# line con 0
Router(config-line)# exec-timeout 5 0
```

## VTY
``` bash
Router# config terminal
Router(config)# line vty 0 4
Router(config-line)# exec-timeout 5 0
```
## AUX

``` bash
Router# config terminal
Router(config)# line aux 0
Router(config-line)# exec-timeout 5 0
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
