- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## sysctl 명령어

``` bash
sysctl [options] [variable[=value] ...]
```

``` bash
-n: 특정 변수의 값을 출력합니다.
-w: 특정 변수의 값을 설정합니다.
-p: /etc/sysctl.conf 파일에 저장된 설정을 읽어서 변수 값을 설정합니다.
-n: 특정키에 대한 값을 보여준다
-A: 테이블형태로 설정가능한 파라미터를 보여준다. -a와 같음
```


``` bash
sysctl -a
```

``` bash
sysctl -n kernel.hostname
```

``` bash
sysctl -w net.ipv4.tcp_rmem='4096 87380 10485760'
sysctl -w net.ipv4.tcp_wmem='4096 87380 10485760'
```

``` bash
sysctl -p
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
