- 마지막 업데이트: 2025-09-26
- 상태: 초안

# 개요
운영체제는 CPU 할당 정책(스케줄링), 자원 경합 해소(교착 상태 대응), 메모리 압박 완화(Thrashing 방지)를 통합적으로 관리해야 일관된 성능을 제공한다. 본 문서는 각 영역의 핵심 알고리즘과 실무 대응 절차를 정리한다.

## 스케줄링
### 선점형 vs 비선점형
| 구분 | 선점형 (SRT, RR, MLFQ 등) | 비선점형 (FCFS, SJF, HRN 등) |
|------|---------------------------|-------------------------------|
| CPU 선점 | 타이머 인터럽트로 가능 | 불가, 프로세스 종료/봉쇄까지 유지 |
| 응답성 | 대화형/실시간 작업에 유리 | 일괄 처리에 유리 |
| 오버헤드 | 컨텍스트 스위칭 비용 존재 | 상대적으로 낮음 |
| 기아 가능성 | 우선순위 역전 가능 → 조정 필요 | 긴 작업 지연, Aging 필요 |

### 대표 알고리즘
- **SRT (Shortest Remaining Time)**: 남은 CPU 버스트가 가장 짧은 프로세스 우선. 평균 대기 시간 최소. 장점: 응답성 높음. 주의: 잦은 선점 (Silberschatz et al., *Operating System Concepts*).
- **HRN (Highest Response Ratio Next)**: `Response Ratio = (Waiting + Service) / Service`. 긴 작업도 대기 시간이 길어지면 선택됨 → 기아 방지 (Williams, 1961).
- **RR (Round Robin)**: 고정 타임슬라이스로 순환. 타임슬라이스 길이는 평균 응답 시간과 컨텍스트 스위칭 오버헤드 사이에서 균형 (UNIX time-sharing 설계 문헌).
- **MLFQ (Multi-Level Feedback Queue)**: 여러 큐와 타임슬라이스 조합으로 I/O bound vs CPU bound 작업을 구분. Aging과 Priority Boost 전략 필수 (BSD, Windows Scheduler).

> 참고: Silberschatz 등, *Operating System Concepts*, Chapter 5.

## 교착 상태(Deadlock) 대응
### 필요 조건 (Coffman 조건)
1. 상호 배제(Mutual Exclusion)
2. 점유와 대기(Hold and Wait)
3. 비선점(No Preemption)
4. 환형 대기(Circular Wait)

### 전략 비교
| 전략 | 방법 | 장점 | 단점 |
|------|------|------|------|
| 예방 (Prevention) | 필요 조건 중 하나를 제거 (예: 자원 순서 지정, 일괄 할당) | 구현 단순, 데드락 없음 | 자원 활용도 저하, 사용자 불편 |
| 회피 (Avoidance) | 은행가 알고리즘 등으로 안전 상태만 허용 | 교착 상태 발생 방지 | 프로세스 자원 요구량 선지식 필요, 계산 비용 |
| 탐지/복구 (Detection & Recovery) | 자원 할당 그래프/행렬 분석, 교착 발생시 프로세스 강제 종료 또는 자원 선점 | 자원 활용 극대화 | 복구 시 데이터 손실 가능, 검사 비용 |
| 무시 (Ignore) | OS가 개입하지 않음 (일부 데스크톱 OS) | 구현 단순 | 교착 상태 지속, 신뢰성 저하 |

**은행가 알고리즘 핵심** (Dijkstra, 1965)
- 안전 순서열(Safe Sequence)이 존재하는지 확인.
- `Need[i][j] = Max[i][j] - Allocation[i][j]`.
- 요청이 현재 가용 자원으로 충족 가능하고, 가상의 할당 후에도 안전 순서열이 유지되면 승인.

## Thrashing (스래싱)
### 정의
- 프로세스들이 충분한 프레임을 확보하지 못해 페이지 부재(Page Fault)가 급증하고, 대부분의 CPU 시간이 페이지 교체에 사용되는 상태.

### 감지 방법
- 페이지 부재율(Page Fault Rate) 모니터링: 임계값 이상이면 스래싱 의심.
- CPU 이용률 감소 + 디스크 I/O 급증 패턴.
- 운영체제 제공 지표 (예: Linux `vmstat`, Windows `Performance Monitor`) 확인.

### 완화 전략
- **워킹셋 모델(Working Set Model)**: 프로세스가 실제로 사용하는 페이지 집합 `W(t, Δ)`을 추적하여 최소 프레임 수를 보장 (Denning, 1968).
- **페이지 부재 빈도(Page Fault Frequency, PFF)**: 목표 범위를 설정하고, 너무 높으면 프레임 추가, 너무 낮으면 회수 (Sevcik & Denning).
- **중기 스케줄러**: 프로세스를 Swap-out하여 메모리 압박 완화.
- **데이터 구조 최적화**: 지역성(Locality)을 높이는 코드/데이터 배치.

> 참고: Andrew S. Tanenbaum, *Modern Operating Systems*, Chapter 3; Red Hat Performance Tuning Guide.

## 실무 체크리스트
- [ ] 스케줄링 정책 변경 시 타임슬라이스와 우선순위 Aging 규칙 문서화.
- [ ] 교착 상태 의심 시 자원 할당 그래프/`lsof`, `fuser`, `Get-ProcessLock` 등의 도구로 원인 추적.
- [ ] 데이터베이스/메시징 시스템은 트랜잭션 타임아웃·락 타임아웃을 설정해 예방.
- [ ] 메모리 모니터링 대시보드에서 페이지 부재율, 스왑 인/아웃을 경고 임계값과 함께 기록.
- [ ] 성능 시험 시 워킹셋 크기를 측정해 최소 메모리 요구량을 산정.

## TODO / 후속 연구
- [ ] 리눅스 CFS와 윈도우 MLFQ 사례 비교 추가.
- [ ] Kubernetes 등 컨테이너 환경에서의 CPU/메모리 스케줄링 참고 자료 링크.
- [ ] 은행가 알고리즘의 행렬 예제 및 코드 스니펫 추가.
- [ ] 워킹셋 기반 튜닝 실습(`pss`, `smem`) 캡처 추가.

## 참고 자료
- Silberschatz, Galvin, Gagne, *Operating System Concepts*, 10th ed.
- Tanenbaum & Bos, *Modern Operating Systems*, 4th ed.
- Red Hat Performance Tuning Guide.
- Microsoft Docs – *Resource Monitor, Memory Working Set*.
