- 마지막 업데이트: 2025-09-26
- 상태: 초안

# 개요
Nmap 스캔 결과는 단발성 CLI 출력으로 끝내지 말고, 타임스탬프·명령어·목표 범위와 함께 파일로 보존해야 재현성과 감사 추적이 확보됩니다. 이 노트는 표준 로깅 워크플로와 파일 구조 예시를 제공합니다.

## 스캔 명령 템플릿
```bash
nmap -Pn -sO 192.168.35.100 \
  -oA logs/nmap/$(date +%Y%m%dT%H%M%S)-os-scan
```
- `-oA`는 같은 접두사로 3종류(`.nmap`, `.gnmap`, `.xml`) 결과를 동시에 저장합니다.
- IP 범위나 포트가 넓다면 `--stats-every 30s`로 진행 상황을 기록하고, `tee`로 콘솔 로그를 함께 남깁니다.

## 실행 전 체크리스트
- [ ] 스캔 목적과 승인 티켓 ID를 `README.md` 또는 로그 파일 헤더에 기록.
- [ ] 대상 네트워크 접근 권한을 확인(특히 외부망/고객망).
- [ ] 최근 스캔 로그와 비교해 변경 포인트를 파악.

## 로그 디렉터리 구조 제안
```
logs/
  nmap/
    20250926T101500-os-scan.nmap
    20250926T101500-os-scan.gnmap
    20250926T101500-os-scan.xml
    20250926T101500-os-scan.cmd.txt
```
- `.cmd.txt` 파일에는 실행 시점 환경 변수를 포함한 명령어·사용자·위치 정보를 남깁니다.

```text
# 20250926T101500-os-scan.cmd.txt
Command: nmap -Pn -sO 192.168.35.100 -oA logs/nmap/20250926T101500-os-scan
RunAt:   2025-09-26T10:15:07+09:00
Host:    recon-jump01
User:    pentest
Notes:   CHG-2025-3921 승인 하에 운영망 점검
```

## 명령 기록 스크립트 예시
```bash
#!/usr/bin/env bash
set -euo pipefail
TARGET="$1"
STAMP=$(date +%Y%m%dT%H%M%S)
BASE="logs/nmap/${STAMP}-full"
mkdir -p "$(dirname "$BASE")"

{
  echo "# Command"
  echo "nmap -Pn -sC -sV ${TARGET} -oA ${BASE}"
  echo
  echo "# Context"
  date --iso-8601=seconds
  hostname
  whoami
} >"${BASE}.cmd.txt"

nmap -Pn -sC -sV "$TARGET" -oA "$BASE"
```

## 로그 샘플
```text
# 2025-09-26T10:15:00Z /usr/local/bin/nmap -Pn -sO 192.168.35.100
Host: 192.168.35.100 ()  Ports: 21/open/tcp//ftp///, 22/open/tcp//ssh///, 80/open/tcp//http///
OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 18.24 seconds

# ndiff 20250919T100000-os-scan.xml 20250926T101500-os-scan.xml
TCP port 443 added to host 192.168.35.100 (service: https)
```
- XML 결과는 `xsltproc` 또는 `nmap -oX -` 파이프와 함께 사용해 HTML 리포트를 생성할 수 있습니다.
- `ndiff` 결과는 변경 요약으로 첨부해 운영팀 승인 문서에 포함합니다.
```
- XML 결과는 `xsltproc` 또는 `nmap -oX -` 파이프와 함께 사용해 HTML 리포트를 생성할 수 있습니다.

## 후처리
- `xsltproc /usr/share/nmap/nmap.xsl 20250926T101500-os-scan.xml -o report.html`
- `ndiff 이전결과 현재결과`로 변경 사항 비교.
- 로그 보관 주기는 보안 정책에 맞춰 6~12개월 단위로 결정합니다.

## TODO / 후속 연구
- [ ] NSE 스크립트 실행 시 로그 증가량 추적 방법 추가.
- [ ] Elastic Stack으로 Nmap XML을 수집하는 파이프라인 예시 작성.

## 참고 자료
- Nmap Reference Guide – <https://nmap.org/book/man-output.html>
- Ndiff 문서 – <https://nmap.org/ndiff/>
- Red Team Field Manual – Nmap Logging Practices.
