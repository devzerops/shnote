# 연구 체크리스트
- 마지막 업데이트: 2025-09-24
- 목적: 참고 문서 링크와 개념 복습 항목을 주제별로 정리

## 참고 문서 & 자료
- **MS 무선 LAN 보안 RADIUS 인프라**: <https://learn.microsoft.com/ko-kr/security-updates/security/20214054>
- **RADIUS & LDAP 연동 개요**: <https://ldap.or.kr/radius%EC%99%80-ldap-1-radius-%EB%9E%80/>
- **추가 사례 연구**
  - <https://totori.tistory.com/489>
  - <https://blog.alyac.co.kr/76>
  - <https://download.ahnlab.com/kr/site/brochure/Brochure_V3_Net_for_Unix_Linux_Server.pdf>

## 도구 & 기술 검토 대상
- VeraCrypt: CLI 생성(`veracrypt --text --create`) 과정 캡처, 볼륨 마운트/해제 로그 확보 → `tech/VeraCrypt.md` 실 로그 시나리오 + 공식 문서 링크 갱신.
- BitLocker: TPM+PIN 정책 점검 스크립트 작성, `manage-bde -status` 결과 스냅샷 → `tech/Bitlock.md` 상세 출력 예시 및 MS Learn 참조.
- Docker Compose: 서비스별 `.env` 샘플과 `docker compose ls` 출력 저장, 헬스체크 동작 확인 → `tech/DockerCompose.md` 점검 로그 샘플 포함.
- Nmap 스캔 예시: `nmap 192.168.35.100 -Pn -sO` 실행 로그와 타임스탬프 보관, false positive 노트 기록 → `tools_notes/scan/nmap/logging.md` 로깅 워크플로·변경 비교 예시 정리.

## 개념 복습 백로그
| 우선순위 | 항목 | 상태 | 메모 |
|----------|------|------|------|
| 높음 | 이진 검색 선행조건 | 완료 | `basic_notes/algorithme/binary_search.md` 정리 완료, [↘︎ 보강 노트](#보강-노트-이진-검색-선행조건) 검토만 유지 |
| 높음 | Link-State 라우팅 프로토콜(OSPF) 세부 | 완료 | `basic_notes/network/7_Layer/L3/protocol/OSPF.md` 영역·코스트 보강 완료 |
| 중간 | ARQ 방식 종류(Stop-and-Wait, Go-Back-N, Selective Repeat) | 완료 | `basic_notes/network/communication/ARQ_PCM_Multiplexing.md` 참고 |
| 중간 | PCM 데이터 전송 순서 | 완료 | `basic_notes/network/communication/ARQ_PCM_Multiplexing.md`에 단계 정리 |
| 중간 | 가상회선 패킷 교환 방식 | 완료 | `basic_notes/network/communication/ARQ_PCM_Multiplexing.md` 가상회선 섹션 |
| 중간 | 다중접속 방식 비교(FDMA/TDMA/CDMA) | 완료 | `basic_notes/network/communication/ARQ_PCM_Multiplexing.md` OFDMA 포함 |
| 중간 | Shannon 표본화 정리 | 완료 | `basic_notes/network/communication/ARQ_PCM_Multiplexing.md` 샤논 요약 |
| 중간 | 스케줄링 알고리즘 (SRT, HRN 등) | 완료 | `OS_notes/Scheduling_Deadlock_Thrashing.md` 스케줄링 섹션 |
| 중간 | 교착상태 해결 전략(Prevention/Avoidance/Detection) | 완료 | `OS_notes/Scheduling_Deadlock_Thrashing.md` 교착 대응 |
| 중간 | Thrashing 정의 및 완화 | 완료 | `OS_notes/Scheduling_Deadlock_Thrashing.md` Thrashing 섹션 |
| 낮음 | 자료사전 반복 기호 `{}` 등 표기 | 완료 | 요약만 유지 |
| 낮음 | 응집도/결합도 등 설계 품질 지표 | 진행중 | 예시 모듈 도식화 |
| 낮음 | 객체지향 동적 모형화 절차 | 미착수 | 시나리오-이벤트 추적 그림 |
| 낮음 | N-S 차트, 폭포수 모형, MTTR 정의 | 진행중 | 프로젝트 관리 항목과 연계 |
| 낮음 | 트랩 발생 원인, 분기 시 레지스터 변화 | 완료 | PC(Program Counter) 변화 사례 기록 |
| 낮음 | 버스 설계용 멀티플렉서 개념 | 미착수 | 다중 버스 구성도 참고 |
| 낮음 | 데이터베이스 설계 단계·정규화 | 진행중 | ERD 예시 포함 예정 |
| 낮음 | 네트워크 모델(DB)과 CODASYL 특징 | 완료 | 네트워크 모델 특성 기재 |
| 낮음 | CNP/PCI-DSS/EMV/NFC/FDS 용어 | 완료 | 카드 결제 보안 용어 정리 |

## 빠른 Q&A 메모
- 위상 변조에서 진폭까지 조절해 전송률을 높이는 기법 → **QAM** (ASK/FSK/PSK는 각각 진폭/주파수/위상 변조).
- 4상 위상 변조에서 800 baud 사용 시 전송 속도 → **1600 bps**.
- HDLC 프레임 순서 → Flag → Address → Control → Information → FCS → Flag.
- 트랩(trap) 발생 원인 예시 → 0으로 나누기 등 예외 상황.
- 분기 명령 시 변경되는 레지스터 → 프로그램 카운터(PC).

## 관심 주제 메모
- 비잔틴 오류, CBDC, 블록체인 & 스마트 컨트랙트
- GSLB 동작 방식
- Preemption(선점) vs Non-preemption 비교
- 프로세스 스케줄링: SJF, SRT, HRN, FIFO 모순 현상(Belady anomaly)
- 기억장치 분할 방식: 고정/단일/동적 분할

향후에는 완료한 항목에 체크박스 및 참조 문서 링크를 추가할 예정입니다.

### 보강 노트: 이진 검색 선행조건
- 입력 컬렉션은 **정렬된 상태**를 유지해야 하며, 정렬 기준이 다중 키일 경우 비교 함수가 일관된 전순서를 제공해야 한다.
- 검색 키와 요소 타입이 동일하거나 비교 연산자를 공유해야 하며, 중복 허용 시 가장 왼쪽/오른쪽 탐색을 위한 후처리를 설계한다.
- 인덱스 접근이 `O(1)`인 배열·슬라이스·랜덤 접근 컨테이너에 적합하고, 연결 리스트처럼 순차 접근만 가능한 구조에는 부적합하다.

```python
from typing import Sequence, TypeVar

T = TypeVar("T")

def binary_search(items: Sequence[T], target: T) -> int:
    """Return the index of target in sorted items or -1 if not found."""
    left, right = 0, len(items) - 1
    while left <= right:
        mid = (left + right) // 2
        probe = items[mid]
        if probe == target:
            return mid
        if probe < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

*테스트 팁*: 경계값(최소·최대 인덱스)과 중복 요소가 있는 케이스를 포함해 `-1` 반환 경로까지 확인한다.

### 보강 노트: OSPF LSDB 및 코스트
**LSDB 동기화 상태 전이**
1. `Down` → `Init`: Hello 수신으로 Neighbor 발견.
2. `Init` → `2-Way`: 이웃 목록에 자신이 확인되면 양방향 통신 확인.
3. `2-Way` → `ExStart/Exchange`: 마스터-슬레이브 협상 후 DD 패킷으로 메타데이터 교환.
4. `Exchange` → `Loading`: 필요 LSA를 `Link State Request`로 요청해 채워 넣음.
5. `Loading` → `Full`: LSDB가 동일해지면 `LSAck`으로 동기화 완료.

**자주 쓰는 코스트 계산 예시(Reference Bandwidth 100Mbps)**

| 링크 대역폭 | 계산식 `Cost = 100Mbps / BW` | 결과 코스트 | 주의 |
|-------------|--------------------------------|-------------|------|
| 10 Mbps     | 100 / 10                       | 10          | 저속 링크, 백업 경로 후보 |
| 100 Mbps    | 100 / 100                      | 1           | 기본값, 동급 링크 균등 코스트 |
| 1 Gbps      | 100 / 1000                     | 0.1 → 1*    |*IOS 기본은 정수 반올림으로 1, 대역폭 갱신 필요 |
| 10 Gbps     | 100 / 10000                    | 0.01 → 1*   |참조 대역폭을 10^5 등으로 조정해야 차등 반영 가능 |

> *`auto-cost reference-bandwidth <Mbps>` 명령으로 고속 링크 환경에 맞게 참조 대역폭을 조정한다.

**운영 체크포인트**
- MTU/Hello/Dead 타이머, 인증 방식이 서로 일치하는지 먼저 검증한다.
- DR/BDR 네트워크에서는 `show ip ospf neighbor`로 `Full/DR` 관계를 확인하고, `ExStart` 상태에 머무르면 MTU 협상 문제를 의심한다.
- LSDB 재동기화 시 `clear ip ospf process`는 서비스 중단을 야기하므로 유지보수 승인 후 수행한다.

### 통신 TL;DR: 오류 제어와 매체 접근
> 상세 정리는 `basic_notes/network/communication/ARQ_PCM_Multiplexing.md` 참고.
**ARQ 비교 (Stop-and-Wait / Go-Back-N / Selective Repeat)**
| 구분 | Stop-and-Wait | Go-Back-N | Selective Repeat |
|------|---------------|-----------|------------------|
| 윈도 크기 | 1 | N | N | 
| 재전송 범위 | 손실 프레임 1개 | 에러 프레임 이후 모두 | 에러 프레임만 | 
| 구현 난이도 | 낮음 | 중간 | 높음 (버퍼·ACK 추적 필요) |
| 지연·대역 효율 | RTT 대비 대역 비효율 | N 증가로 개선 | 고효율, 고복잡 |

**PCM(펄스 부호 변조) 전송 순서**
1. **표본화(Sampling)**: 아날로그 신호를 주기 `T_s`로 측정.
2. **양자화(Quantization)**: 샘플 값을 근사 값으로 매핑.
3. **부호화(Encoding)**: 양자화된 값을 비트열로 변환.
4. (옵션) **라인 코딩/전송**: NRZ, RZ 등 전송 부호로 매핑 후 전송.

**가상회선 패킷 교환 절차**
1. **Call Setup**: 라우터 간 경로 예약, VC ID 교환.
2. **Data Transfer**: 순서 보장, 동일 VC ID 유지.
3. **Teardown**: 연결 해제 메시지로 자원 반환.
→ 연결 지향 QoS 제공, 장애 시 재설정 필요.

**다중접속 비교**
| 방식 | 자원 분할 | 장점 | 제약 |
|------|-----------|------|------|
| FDMA | 주파수 대역 | 간단, 동시성 높음 | 대역 고정, 가드밴드 필요 |
| TDMA | 시간 슬롯 | 디지털 표준에 적합 | 동기화 비용, 지터 |
| CDMA | 코드 | 주파수 재사용, 보안성 | 코드 관리 복잡, 근접 효과 |
| OFDMA | 서브캐리어+시간 | LTE/5G 다운링크, QoS 미세화 | 스케줄링·CFO 보정 필요 |

**Shannon-나이퀴스트 표본화**
- 최대 주파수 `f_max`인 아날로그 신호 → 표본 주파수 `f_s ≥ 2 f_max` 필요.
- 예: 4 kHz 음성 대역 전화선은 `f_s = 8 kHz`, 샘플당 8비트면 `64 kbps`.
- SNR 기반 채널 용량: `C = B log2(1 + S/N)`로 대역폭·잡음 Trade-off 확인.

### OS TL;DR: 스케줄링·동기화
> 상세 정리는 `OS_notes/Scheduling_Deadlock_Thrashing.md` 참고.
**선점 vs 비선점 스케줄링**
| 항목 | 선점형 (SRT, RR, MLFQ) | 비선점형 (FCFS, SJF, HRN) |
|------|------------------------|---------------------------|
| CPU 선점 | 가능, 타이머 인터럽트 활용 | 불가, 프로세스 종료/봉쇄까지 유지 |
| 응답성 | 높음 (대화형 적합) | 낮음 (일괄 처리 적합) |
| 오버헤드 | 컨텍스트 스위칭 비용 존재 | 낮음 |
| 기아 위험 | 우선순위 조정 필수 | 긴 작업 지연 가능 |

**대표 알고리즘 메모**
- `SRT`: 남은 시간 가장 짧은 작업 우선, 대기 시간 최소화.
- `HRN`: (대기시간 + 서비스시간)/서비스시간 비율 최대값 선택, 기아 방지.
- `RR`: 고정 타임퀀텀 순환, 응답시간 보장.
- `MLFQ`: 큐별 타임퀀텀·우선순위를 조정해 장·단기 작업 분리.

**교착상태 대응 전략**
| 전략 | 핵심 아이디어 | 장단점 |
|-------|----------------|--------|
| 예방(Prevention) | 상호배제·점유와대기 등 필요조건 제거 | 단순하지만 자원 활용도 저하 |
| 회피(Avoidance) | 은행가 알고리즘 등으로 안전 상태만 허용 | 추가 정보 요구, 계산비용 |
| 탐지/복구(Detection) | 자원 할당 그래프·탐지 알고리즘으로 확인 후 선점/중단 | 교착 허용, 복구 비용 필요 |

**Thrashing 탐지·완화**
- 징후: 페이지 부재율 급증, CPU 이용률 급락, 스왑 I/O 과다.
- 완화: 워킹셋 모델로 프로세스 메모리 상한 조정, 페이지 부재 빈도(PFF) 기반 조절, 중기 스케줄러로 일부 프로세스 Swap-out.
- 튜닝: 캐시 친화적 데이터 구조, I/O 바운드/CPU 바운드 균형 잡힌 작업 큐 구성.
