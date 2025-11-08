- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
``` powershell
1..255 | % {echo "192.168.35.$_"; ping -n 1 -w 100 192.168.35.$_ | Select-String ttl
```

``` powershell
1..1024 | % {echo ((NEW-Objecr Net.Sockets.TcpClient).Connect("192.168.35.128", $_)) "Open - $_"} 2>$null
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
