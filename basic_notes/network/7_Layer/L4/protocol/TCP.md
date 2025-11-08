- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
TCP(Transmission Control Protocol)는 신뢰성 있는 양방향 연결을 제공하는 L4 전송 프로토콜입니다. 연결 설정/해제, 순서 제어, 흐름 제어, 혼잡 제어를 통해 애플리케이션에 안정적인 데이터 스트림을 전달합니다.

## 헤더 필드 요약
| 필드 | 크기 | 설명 |
|------|------|------|
| Source / Destination Port | 16bit | 송수신 애플리케이션 식별 |
| Sequence Number | 32bit | 스트림 내 바이트 순서 지정, 손실/순서 제어 |
| Acknowledgment Number | 32bit | 다음에 기대하는 시퀀스 번호, 누적 ACK |
| Data Offset | 4bit | 헤더 길이(옵션 포함) |
| Flags | 6bit | URG, ACK, PSH, RST, SYN, FIN 제어 비트 |
| Window Size | 16bit | 가용 수신 버퍼 크기 광고(흐름 제어) |
| Checksum | 16bit | 의사 헤더 포함 오류 검출 |
| Urgent Pointer | 16bit | URG 플래그 사용 시 긴급 데이터 위치 |
| Options | 가변 | MSS, SACK, Timestamp 등 기능 확장 |

## 주요 플래그
- **SYN**: 연결 설정, 시퀀스 번호 초기화 (`SYN` → `SYN/ACK` → `ACK`).
- **FIN**: 연결 종료, 양방향으로 각각 전송.
- **ACK**: 모든 세그먼트에서 설정, 누락 확인.
- **RST**: 비정상 세션 리셋.
- **PSH/URG**: 버퍼 우회/긴급 데이터 처리.

# 핵심 개념
- **연결 관리**: 3-way handshake로 세션 생성, 4-way handshake로 종료.
- **흐름 제어**: 슬라이딩 윈도우, Zero Window Probe로 수신 측 혼잡 감지.
- **혼잡 제어**: Slow Start, Congestion Avoidance, Fast Retransmit/Recovery로 네트워크 상태에 적응.
- **신뢰성**: 누적 ACK, 재전송 타이머(RTO), Selective ACK 옵션으로 데이터 손실 대응.
- **세그먼트 재조립**: 순서 보장을 위해 수신 버퍼에서 재정렬 후 애플리케이션에 전달.

# 실무/시험 포인트
- 방화벽 정책 작성 시 상태 기반(Stateful) 검사가 TCP 플래그 조합을 활용함을 이해해야 한다.
- SYN Flood 대응: SYN Cookies, conntrack 제한, `tcp_syncookies` 튜닝 등 활용.
- 트러블슈팅 시 `netstat`, `ss`, `tcpdump`로 상태 코드(`SYN_SENT`, `TIME_WAIT`)를 확인한다.
- 시험에서는 3-way handshake 순서, 플래그 의미, 윈도우 사이즈 계산 문제가 빈출.

# TODO / 후속 연구
- 혼잡 제어 상태 기계 그래프 도식화 후 `drawio/`에 저장.
- Wireshark 캡처 예시 추가(`tcp.flags.syn==1` 필터 등).
- QUIC과 비교한 차이점(전송 계층 vs 애플리케이션 계층)을 별도 문서로 정리.

# 참고 자료
- RFC 9293 – Transmission Control Protocol.
- Stevens, *TCP/IP Illustrated Vol.1* – 세션 수립 및 혼잡 제어.
- Wireshark TCP 분석 가이드.
