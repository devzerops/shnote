- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
기본적인 Nmap 사용 패턴을 빠르게 참고하기 위한 명령 모음입니다. 네트워크 범위 지정, 핑 스캔, 포트 스캔, NSE 활용 예시를 정리했습니다.

## 대상 지정 예시
```bash
nmap 192.168.1.100          # 단일 호스트
nmap 192.168.1.100-150      # IP 범위
nmap 192.168.1.100,110      # 비연속 호스트
nmap 192.168.1.0/24         # CIDR 블록
nmap -iR 100                # 무작위 100개 호스트
```

## 기본 스캔 예시
```bash
nmap localhost              # Well-known 포트 빠른 확인
nmap -sn 192.168.1.0/24     # Ping Sweep (호스트 탐지)
nmap -sS -p 22,80 10.0.0.5  # SYN 스캔 + 지정 포트
nmap -O 10.0.0.5            # OS 탐지
nmap -sV --version-light 10.0.0.5  # 서비스 버전 확인
```

## NSE 활용
```bash
nmap --script vuln 10.0.0.5
nmap -p80,443 --script http-headers,http-title --open example.com
nmap --script-updatedb      # 스크립트 DB 최신화
```

# 핵심 개념
- CIDR, 범위, 목록 등 다양한 대상 지정 방식을 조합해 스캔 범위를 제어합니다.
- `-sn`은 포트 스캔을 생략하고 호스트 가용성만 확인합니다.
- NSE 스크립트는 카테고리(예: `safe`, `vuln`)로 묶여 있으며, 업데이트 후 사용해야 최신 취약점을 탐지할 수 있습니다.

# 실무/시험 포인트
- 침해 대응에서는 `nmap -sS -Pn -T4 -p- --open` 등으로 확장 범위를 탐색한 후, 결과를 `-oA`로 저장해 증적을 남깁니다.
- 사내/외부 스캔은 반드시 허가된 범위에서만 수행하며, 로그 남김(`--packet-trace`, `--reason`)으로 감사 대응을 준비합니다.
- 자격시험(OSCP, 정보보안 기사)에서 IP 범위 지정, ping sweep, NSE 사용법 문제를 자주 다룹니다.

# TODO / 후속 연구
- `masscan`과의 비교 표 작성.
- IPv6 스캔(`-6`, `--script ipv6-*`) 예시 추가.
- CSV/HTML 보고서 변환(`xsltproc`, `grep`) 스크립트 공유.

# 참고 자료
- Nmap Reference Guide – [https://nmap.org/book/man.html](https://nmap.org/book/man.html).
- Nmap Cookbook – 호스트 탐지/포트 스캔 레시피.
