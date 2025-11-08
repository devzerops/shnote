- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
tcpdump는 리눅스/유닉스 환경에서 패킷을 캡처하고 필터링할 수 있는 CLI 네트워크 분석 도구입니다. libpcap 기반으로 동작하며, 수집한 PCAP은 법적 증거로 활용될 수 있어 정확한 타임스탬프와 필터 설정이 중요합니다.

## 기본 사용 예시
```bash
# 인터페이스 eth0에서 TCP 80번 포트 패킷 10개 캡처 후 종료
tcpdump -i eth0 tcp port 80 -c 10

# 결과를 PCAP 파일로 저장
tcpdump -i eth0 -w web.pcap

# 저장 파일을 읽어 사람이 보기 쉬운 형태로 출력
tcpdump -r web.pcap -nn -tttt
```

## 주요 옵션
| 옵션 | 설명 |
|------|------|
| `-i <iface>` | 캡처할 인터페이스 지정 (`-D`로 목록 확인) |
| `-nn` | IP/포트를 숫자 그대로 표시 |
| `-c <count>` | 지정한 패킷 수만 캡처 |
| `-s <snaplen>` | 캡처 길이 지정(기본 262144). 전체 패킷 필요 시 `-s 0` |
| `-A` / `-X` | ASCII / Hex+ASCII 출력 |
| `-w <file>` | PCAP 파일로 저장 |
| `-r <file>` | 저장된 PCAP 읽기 |
| `-Z <user>` | 캡처 후 권한 강등 |

## 필터 문법 요약
- **기본 구조**: `[proto] [direction] [type] value` (예: `tcp src port 443`).
- 논리 연산: `and`, `or`, `not`.
- 예시
  - `host 192.168.1.10 and not port 22`
  - `tcp[tcpflags] & tcp-syn != 0`
  - `ether proto 0x0806` (ARP)

# 핵심 개념
- 캡처 시점 정확도를 위해 NTP/PTP 동기화와 타임존 설정이 선행되어야 한다 (RFC 5905 참조).
- 스니핑 권한은 root 또는 `CAP_NET_RAW`가 필요하며, GDPR/PCI-DSS 등 규정에 따라 로그 접근 제어가 필수다.
- 고속 링크 캡처 시에는 `-s 0 -B <buffer>`로 버퍼 크기를 늘리고, ring buffer(`-C`/`-G`)를 활용해 파일 단위를 분할한다.
- IEEE 802.11/802.1Q 환경에서는 모니터 모드/태그 캡처를 위한 드라이버 지원을 확인한다.

# 실무/시험 포인트
- 침해 대응에서는 `tcpdump -nn -tttt -v` 형태로 시간·플래그·주소를 모두 확인한 후 Wireshark로 심층 분석한다 (SANS SEC503 권고).
- `tcpdump -i any`는 모든 인터페이스를 대상으로 하지만, 로컬 루프백 패킷은 OS별로 포함 여부가 다르므로 확인이 필요하다.
- 필터 최적화를 통해 필요 패킷만 수집하면 추후 분석 시간을 줄일 수 있다. 예) `tcpdump 'tcp port 443 and tcp[tcpflags] & (tcp-syn|tcp-fin) != 0'`.
- 정보보안 기사, 네트워크 분석 관련 시험에서 BPF 문법과 옵션 해석 문제가 출제된다.
- 장기 캡처는 GDPR/PII 요구사항을 충족하기 위해 탈식별화 절차와 보관 기한을 명시한다.

# TODO / 후속 연구
- ring buffer(`-w file -C size -W count`) 예시 추가.
- IPv6, VLAN, MPLS 캡처 필터 예시 정리.
- Windows용 WinDump 설치/사용법 링크 추가.
- `pf_ring`, `AF_PACKET` 등 고성능 캡처와의 차이 비교.

# 참고 자료
- tcpdump/pcap 공식 문서 – [https://www.tcpdump.org/manpages/](https://www.tcpdump.org/manpages/).
- Stevens, *UNIX Network Programming* – Packet Capture 절.
- SANS SEC503 Packet Analysis Cheat Sheet.
- RFC 5905 – NTPv4 Specification.
