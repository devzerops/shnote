- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
Wireshark는 GUI 기반의 대표적인 패킷 분석 도구로, 수백 가지 프로토콜을 자동 해석하고 디코딩합니다. 문제 구간 식별, 프로토콜 동작 검증, 침해 조사에 폭넓게 활용됩니다.

## 주요 장점과 한계
- 장점: 풍부한 프로토콜 디코더, 강력한 디스플레이 필터, 통계/흐름 그래프 제공.
- 한계: 트래픽 점유율, 장시간 분석에는 비효율적이므로 CLI 도구(tshark)나 NetFlow/IDS와 병행 필요.

# 핵심 개념
- **캡처 vs 디스플레이 필터**: `tcp port 80`(캡처)와 `tcp.port == 80`(디스플레이)처럼 문법이 다르므로 상황에 맞게 구분 사용합니다.
- **컬러링 룰**: 기본 룰 또는 사용자 정의 룰을 적용해 비정상 패킷을 강조합니다.
- **Follow TCP/UDP Stream**: 흐름을 재조합해 요청/응답을 순차적으로 확인합니다.
- **Expert Info**: 재전송, Window Zero 등 경고를 한눈에 제공해 문제 지점을 빠르게 찾을 수 있습니다.
- **보안 주의**: 루트 권한으로 직접 실행하지 말고 `dumpcap` 캡처 권한만 부여한 그룹을 사용합니다.

# 실무/시험 포인트
- 침해 조사에서는 `Statistics > Conversations`로 세션별 통계를 확인한 뒤, 필터를 적용해 세부 페이로드를 분석합니다.
- 흔한 디스플레이 필터 조합: `ip.addr == 10.0.0.5`, `tcp.flags.syn == 1 && tcp.flags.ack == 0`, `dns.qry.name contains "example.com"`.
- TLS 트래픽 분석 시 `ssl.handshake.type == 1`로 ClientHello를 확인하고, JA3 핑거프린트는 스크립트 또는 Zeek와 연동해 분석합니다.
- 시험에서는 필터 문법, Follow Stream 기능, 캡처 파일 관리 방법을 자주 묻습니다.

# TODO / 후속 연구
- `dumpcap` CLI 캡처 예제와 파일 롤링 옵션 추가.
- 패킷 컬러링 룰(.colr) 공유 및 import 방법 정리.
- HTTP2, QUIC 등 최신 프로토콜 분석 팁 정리.

# 참고 자료
- Wireshark User Guide – [https://www.wireshark.org/docs/](https://www.wireshark.org/docs/).
- Laura Chappell, *Wireshark Certified Network Analyst*.
- `man dumpcap`, `man tshark` – CLI 캡처 옵션.
