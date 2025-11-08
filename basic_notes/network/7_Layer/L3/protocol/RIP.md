- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
RIP(Routing Information Protocol)는 거리 벡터 기반 IGP로, 홉 수를 메트릭으로 사용하는 간단한 라우팅 프로토콜입니다. 최대 15홉까지 지원해 소규모 네트워크에 적합하며, 주기적으로 전체 라우팅 테이블을 브로드캐스트합니다.

# 핵심 개념
- **메트릭**: 홉 수가 유일한 비용 기준이며, 16홉은 무한대(도달 불가)로 간주합니다.
- **업데이트 주기**: 기본 30초마다 전체 라우팅 테이블을 전송하며, `triggered update`로 변화 시 즉시 알립니다.
- **버전**: RIP v1(클래스풀, 브로드캐스트)과 RIP v2(클래스리스, 멀티캐스트 224.0.0.9, 인증 지원).
- **Split Horizon/Poison Reverse**: 루프 방지 기법으로, 들어온 인터페이스로 다시 경로를 광고하지 않거나( Split Horizon) 무한대로 독에 물린 값(16)을 광고합니다.
- **Hold-down Timer**: 불안정한 경로를 일정 시간 동안 사용하지 않게 막아 루프를 줄입니다.

# 실무/시험 포인트
- 규모가 작은 지사 네트워크, 임시 구축 환경에서 여전히 사용되며, OSPF/BGP 대비 설정이 간단합니다.
- 라우팅 루프와 수렴 속도 문제 때문에, 백본 환경에서는 OSPF/ISIS로 전환하는 것이 일반적입니다.
- 시험에서는 메트릭 한계(15홉), 루프 방지 기법(Split Horizon, Poison Reverse), v1/v2 차이, 업데이트 주기를 묻는 경우가 많습니다.
- 장비 구성이 섞여 있는 경우, RIP v1은 VLSM을 지원하지 않으므로 v2 사용을 권장합니다.

# TODO / 후속 연구
- RIPng(IPv6) 설정 예시와 v2 차이 비교 표 추가.
- `debug ip rip` 출력 예제를 캡처해 수렴 과정을 설명.
- Split Horizon 비활성화가 필요한 토폴로지(예: Frame Relay Hub-and-Spoke) 사례 추가.

# 참고 자료
- RFC 2453 – RIP Version 2.
- Cisco IOS Configuration Guide – RIP 구성 및 트러블슈팅.
- Juniper TechLibrary – RIPng 구성.
