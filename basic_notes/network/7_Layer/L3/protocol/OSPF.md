- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
OSPF(Open Shortest Path First)는 링크 상태 기반 IGP로, SPF(Dijkstra) 알고리즘을 이용해 최단 경로 트리를 계산합니다. 영역(Area) 개념과 LSA(Link State Advertisement)를 통해 대규모 네트워크에서도 빠른 수렴과 확장성을 제공합니다.

## 작동 흐름 요약
1. **Neighbor Discovery**: Hello 패킷으로 인접 라우터를 탐색하고 파라미터를 교환합니다.
2. **Adjacency 형성**: DR/BDR 선출(멀티액세스 네트워크) 후 Database Description/DD 패킷 교환.
3. **LSA 동기화**: Link State Request/Update/Ack 패킷으로 LSDB를 동일하게 유지.
4. **SPF 계산**: 라우터는 자신의 LSDB를 기반으로 SPF 트리를 계산해 Routing Table을 생성합니다.

# 핵심 개념
- **LSA 유형**: Type1(라우터 LSA), Type2(네트워크 LSA), Type3(요약 LSA), Type4(요약 ASBR), Type5(AS 외부), Type7(NSSA) 등. Flooding 스코프가 다르므로 영역 경계에서 필터링을 고려한다.
- **영역 구조**: 백본(Area 0)을 중심으로 스터브, Totally Stubby, NSSA, Totally NSSA를 조합해 LSDB 크기와 경계 기기 부담을 줄인다.
- **코스트(Cost)**: 기본 참조 대역폭 100Mbps에서 `Cost = Reference_BW / Interface_BW`로 계산. 고속 링크에서는 참조 대역폭을 상향 조정해야 차이가 반영된다.
- **라우터 역할**: Internal, Backbone, ABR(Area Border Router), ASBR(Autonomous System Boundary Router) 각각이 Flooding 범위와 요약 책임을 나눈다.
- **타이머**: 기본 Hello 10s(브로드캐스트), Dead 40s. 타이머, 인증, MTU가 다르면 Adjacency가 `Init` 단계에서 중단된다.
- **LSDB 동기화 상태**: `Down → Init → 2-Way → ExStart → Exchange → Loading → Full`. `ExStart`에서 멈추면 MTU 또는 Master/Slave 협상 문제를 의심한다.

## 영역 설계 가이드
- Cisco *OSPF Design Guide* 기준으로, **두 개 이상의 Area**가 존재할 때는 Area 0과의 물리적/논리적 연결을 유지해야 한다. 끊어질 경우 Virtual Link를 검토하되 장기 해법은 토폴로지 조정이다.
- Stub/Totally Stub 영역은 Type5 LSA를 차단해 라우팅 테이블을 축소하지만, 외부 경로가 필요한 경우 NSSA(외부 경로를 Type7으로 전달)로 전환한다.
- ABR는 영역마다 별도 LSDB를 보유하므로 CPU/메모리 여유를 고려한 배치가 필요하며, Summarization을 통해 외부 영역에 불필요한 상세 경로를 숨길 수 있다.
- SoO(Site of Origin) 태그나 Route Tag를 활용해 백도어 경로가 요약을 통해 재분배되지 않도록 한다.

## 코스트 튜닝
- 고속 링크(>100Mbps)가 포함된 네트워크에서는 `auto-cost reference-bandwidth <Mbps>` 명령으로 참조 대역폭을 10,000 혹은 100,000으로 늘려 차등을 반영한다.
- 멀티패스 환경에서 동일 코스트의 경로가 존재하면 ECMP가 활성화되므로, 특정 경로를 우선하려면 인터페이스 코스트를 수동 조정한다.
- 링크 품질이 낮거나 임시 백업 회선은 코스트를 명시적으로 높여 SPF 계산 시 마지막 선택으로 남긴다.

### 코스트 예시(참조 대역폭 100Mbps)
| 링크 유형 | 대역폭 | 계산 코스트 | 실무 메모 |
|-----------|--------|-------------|-----------|
| FastEthernet | 100 Mbps | 1 | 기본값, 균등부하 시 다수 경로 동등 처리 |
| GigabitEthernet | 1 Gbps | 0.1 → 1* | 참조 대역폭 상향 필요, IOS 정수 반올림 |
| 10GigE | 10 Gbps | 0.01 → 1* | `auto-cost 100000` 적용 후 10 → 1 재조정 |
| T1 | 1.544 Mbps | ≈ 64 | 저속 회선, 백업 경로 지정 |

> *IOS는 정수 코스트만 사용하므로, 참조 대역폭을 상향해 고속 링크 간 차이를 반영해야 한다.

## 트러블슈팅 체크리스트
- Neighbor가 `Init` 상태에서 멈출 때: Hello/Dead 타이머, 인증, Area ID, MTU 불일치 확인.
- `Exchange` 또는 `Loading`에서 멈출 때: LSDB 크기 초과, LSA 타입 미지원, Database Description 재전송 여부 확인.
- 빈번한 SPF 재계산: `show ip ospf` → SPF Count 확인, `timers throttle spf` 조정 및 LSA Flooding 원인 파악.
- 외부 경로 비정상: ASBR 재분배 설정 확인, `default-information originate` 조건 검토, NSSA에서 Type7 → Type5 변환 여부 확인.

# 실무/시험 포인트
- DR/BDR는 멀티액세스 네트워크(예: Ethernet)에서만 선출되며, 포인트투포인트 링크에서는 필요하지 않습니다.
- MTU, 인증, 영역 ID가 다르면 인접성(Adjacency)이 형성되지 않으므로 기본 점검 항목으로 확인합니다.
- LSA Flooding 문제 발생 시 `ip ospf database` 확인 후 `clear ip ospf process`로 재수렴을 유도하되, 유지보수 시간대에 수행합니다.
- Stub/NSSA 영역은 외부 LSA(Type5)를 차단하여 라우팅 테이블을 단순화하지만, 있을 경우 ASBR을 배치할 수 없습니다(NSSA 예외 포함).
- 자격증(예: CCNA, CCNP ENARSI)에서는 LSA 유형 매칭, DR 선출 조건, SPF 계산 순서를 자주 묻습니다.

# TODO / 후속 연구
- OSPFv2 vs OSPFv3 비교 표 작성(주소 체계, 인증 방식, LSAs).
- Packet capture 예시를 정리해 Wireshark 필터(`ospf`) 분석 캡처를 첨부.
- `basic_notes/network/7_Layer/L3/protocol/RIP.md`와 비교 섹션 추가.

# 참고 자료
- Cisco, *OSPF Design Guide* – DR/BDR 및 영역 설계 베스트 프랙티스.
- RFC 2328 – OSPF Version 2 Specification.
- RFC 5340 – OSPF for IPv6(OSPFv3).
- Juniper TechLibrary – OSPF 트러블슈팅 명령 예시.
