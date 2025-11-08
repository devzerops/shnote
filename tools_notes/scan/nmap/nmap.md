- 마지막 업데이트: 2025-09-25
- 상태: 검토중

# 개요
Nmap(Network Mapper)은 호스트 발견, 포트 스캔, 서비스/버전 탐지, 방화벽 우회 테스트까지 지원하는 대표적 네트워크 진단 도구입니다. NSE(Nmap Scripting Engine)를 통해 취약점 진단과 자동화도 수행할 수 있습니다.

## TL;DR
- 스캔 흐름은 `호스트 발견 → 포트 스캔 → 서비스/버전 → NSE 스크립트` 순으로 진행하면 분석이 쉽습니다.
- 결과는 `-oA`로 통합 저장하고, `--reason`, `--packet-trace`로 방화벽/필터 동작을 확인하세요.
- Mermaid 다이어그램과 실습 예제 출력으로 전체 워크플로와 기본 사용법을 빠르게 파악할 수 있습니다.

## 기본 사용 패턴
```bash
nmap [스캔타입] [옵션] <대상>
```
예: `nmap -sS -p 22,80 -O -T4 192.168.10.0/24`

## 주요 스캔 유형
| 옵션 | 설명 | 특징 |
|------|------|------|
| `-sS` | SYN(Half-open) 스캔 | 빠르고 은밀, 기본값 |
| `-sT` | TCP Connect 스캔 | 표준 소켓 사용, 권한 불필요 |
| `-sU` | UDP 스캔 | 느리지만 UDP 서비스 확인 |
| `-sA` | ACK 스캔 | 방화벽 규칙 추정 |
| `-sN`/`-sF`/`-sX` | NULL/FIN/Xmas | RFC 규칙을 이용한 스텔스 스캔 |
| `-sV` | 서비스 버전 탐지 | 애플리케이션 버전 판별 |
| `-O` | OS Fingerprint | TCP/IP 스택 특성 분석 |

## 포트/출력 옵션
- `-p`: 포트 지정(`-p 1-1024`, `-p T:80,443,U:53`).
- `-Pn`: 핑 스킵, 방화벽이 ICMP 차단하는 환경에서 사용.
- `-T0~5`: 타이밍 템플릿(0=Paranoid, 5=Insane). 시험에서는 `-T4`가 일반적.
- `-oN`/`-oX`/`-oG`/`-oA`: 결과 저장(일반/ XML/ Grepable/ 전체).

## NSE(Nmap Scripting Engine)
- 스크립트 위치: `/usr/share/nmap/scripts`.
- 실행 방식: `nmap --script=<category|script-name> <target>`.
- 주요 카테고리: `default`, `safe`, `auth`, `vuln`, `discovery`, `brute` 등.
- 예시: `nmap -sV --script=vuln 10.0.0.5`.

## 실습 예제
```bash
# 빠른 SYN 스캔 + 버전 탐지 + 결과 저장
nmap -sS -sV -T4 -oA scans/web 192.168.10.15

# 방화벽 추적을 위한 ACK 스캔
nmap -sA --reason 203.0.113.20

# NSE default 카테고리 실행
nmap -sV --script=default 192.168.10.15 -oN scans/default.log
```

예상 출력 일부:
```
Nmap scan report for 192.168.10.15
Host is up (0.00043s latency).
Not shown: 996 closed tcp ports (reset)
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 8.4p1 Ubuntu 5 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http    nginx 1.18.0 (Ubuntu)
443/tcp open  https   nginx 1.18.0 (Ubuntu)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
```

# 핵심 개념
- 호스트 발견(Ping 스캔) → 포트 스캔 → 서비스 탐지 → NSE 실행 순서로 이해하면 결과 분석이 쉽다.
- 방화벽이 있는 환경에서는 `-sA`, `--reason`, `--packet-trace`로 필터링 동작을 역추적한다.
- 성능 최적화 시 타이밍 템플릿과 병렬도(`--min-parallelism`, `--min-rate`)를 조정하되, IDS 회피 목적이라면 너무 높은 값은 피한다.

# 실무/시험 포인트
- 보안 점검 보고서는 `-oA`로 표준화된 결과를 남긴 후 `xsltproc` 또는 `grep`으로 후처리한다.
- `-sV`와 `--script vuln` 조합은 빠른 취약점 탐색에 유용하지만 오탐 가능성을 고려해 수동 검증이 필요하다.
- 자격시험(OSCP, CEH 등)에서는 스캔 플래그 해석, NSE 사용법, 타깃 세그먼트 스캔 전략을 많이 묻는다.

```mermaid
flowchart LR
    Discover[호스트 발견 (Ping 스캔)] --> Ports[포트 스캔 (TCP/UDP)]
    Ports --> Service[서비스/버전 탐지]
    Service --> NSE[NSE 스크립트 실행]
    NSE --> Output[결과 저장/보고 (-oA)]
    Ports --> Firewall[방화벽 추적 (--reason, --packet-trace)]
    Firewall --> Tuning[타이밍/병렬 조정 (-T, --min-rate)]
```

# TODO / 후속 연구
- [ ] `nmap --script-updatedb` 사용 시나리오와 스크립트 작성 기초 정리.
- [ ] 화이트리스트 네트워크에서의 대규모 스캔 자동화 스크립트 작성.
- [ ] Wireshark로 스캔 패턴을 캡처하여 플래그별 패킷 차이를 시각화.

# 참고 자료
- Gordon Lyon, *Nmap Network Scanning*.
- 官方 Docs – [Nmap Reference Guide](https://nmap.org/book/man.html).
- [NSE Documentation](https://nmap.org/book/nse.html).
