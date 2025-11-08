- 마지막 업데이트: 2025-09-24
- 상태: 초안

# EDR(Endpoint Detection and Response) 앤드포인트 탐지 및 대응

## 개요
- 엔드포인트 장치(워크스테이션, 서버, 모바일)에 대한 실시간 위협 감지·조치 플랫폼.
- 중앙 집중 수집/분석으로 포렌식, 위협 헌팅, 대응 자동화를 지원.

## 핵심 개념 / 절차
- **데이터 수집**: 프로세스 실행, 레지스트리 변경, 파일/네트워크 활동 로그를 에이전트가 전송.
- **분석 엔진**: 시그니처, 행동 기반 탐지, ML 모델, 위협 인텔 매칭을 병행.
- **대응 플로우**: 경보 → 분석가 triage → 격리/차단/롤백 → 교정(E&O) → 보고서.
- **포렌식 재생**: 타임라인 재구성, 메모리 덤프 및 IOC pivot 기능.

### 주요 솔루션 비교
| 항목 | Microsoft Defender for Endpoint | CrowdStrike Falcon | SentinelOne Singularity |
|------|--------------------------------|--------------------|-------------------------|
| 배포 방식 | SaaS 기반, Windows/Linux/macOS 에이전트 | SaaS 기반, 다중 플랫폼 에이전트 | SaaS + 온프렘 하이브리드, macOS/Linux 포함 |
| 탐지 엔진 | 행동 분석 + 클라우드 AI + TI | Threat Graph, ML 기반 행위 탐지 | AI 기반 자동 판단 + Storyline 타임라인 |
| 대응 기능 | 자동 조사(ATP), 네트워크 격리, Live Response | RTR(Remote Response), 아이솔레이션, IOC 푸시 | 1-click 롤백, 자동 힐링, 스크립트 실행 |
| 통합 | Microsoft 365 Defender, Sentinel, Azure Arc | Falcon Fusion, SIEM/SOAR 커넥터 | SOAR/ITSM API, Cloud Funnel 로그 수집 |
| 라이선스 고려 | Microsoft E5/Security SKU | 모듈형 구독(Prevent/Insight/Overwatch 등) | Core/Control/Complete 티어 |

## 실무 / 시험 포인트
- EPP(전통 AV)와 차별점: EDR은 행위·메모리 기반 탐지, 공격 체인 추적, 대응 자동화에 중점.
- MITRE ATT&CK 매핑: 주요 제품은 탐지 규칙과 경보를 ATT&CK 단계와 연결해 보고한다.
- 배포 전 고려: 에이전트 호환성, 네트워크 대역폭, SOC 통합(API, Syslog), 개인정보 이슈.
- 탐지 룰 작성 시에는 `ProcessCreation` + `NetworkConnection` 이벤트를 결합해 Living-off-the-Land 공격을 식별하고, 허용 리스트(서명 정보, 경로)를 명확히 관리한다.

### 예시 탐지 룰 (Sigma/KQL)
```yaml
title: Suspicious PowerShell Download Cradle
logsource:
  product: windows
  service: security
  category: process_creation
detection:
  selection:
    Image|endswith: \powershell.exe
    CommandLine|contains:
      - "IEX"
      - "New-Object Net.WebClient"
  condition: selection
falsepositives:
  - 관리형 PowerShell 스크립트 배포
level: high
tags:
  - attack.t1059.001
```

```kql
DeviceProcessEvents
| where FileName =~ "rundll32.exe"
| where ProcessCommandLine has "javascript:" or ProcessCommandLine has "url.dll"
| join kind=inner (DeviceNetworkEvents
    | where RemoteUrl in ("pastebin.com", "raw.githubusercontent.com"))
  on DeviceId, InitiatingProcessId, Timestamp
| extend ATTACKTechnique = "T1218"
```
- 룰 추가 후에는 1~2주 동안 조정 기간을 두고, 운영팀이 허용해야 할 예외를 `AllowedHash`, `AllowedPublisher` 리스트로 관리한다.

### 자동화 플레이북 운영 시 주의점
- **격리 오탐 (False Containment)**: 재택 근무 단말, OT 장비 등 격리 시 즉시 업무 중단이 발생하는 자산은 `Critical Asset` 태그로 관리하고, 플레이북에서 승인 단계를 추가한다.
- **롤백 실패**: 프로세스 강제 종료 후 파일 원복이 필요한 시나리오(예: 라인업 비즈니스 앱)가 있으므로 `Application Allowlist`와 `Rollback Exclusion`을 분리해 관리한다.
- **권한 상승 스크립트**: SOAR에서 원격 PowerShell을 실행할 때는 `Just Enough Administration` 계정과 서명된 스크립트를 사용해 남용을 방지한다.
- **경보 폭주 대비**: 동일 IOC로 5회 이상 격리 이벤트가 발생하면 자동 격리를 중단하고 분석가 승인으로 전환하는 `Circuit Breaker` 조건을 둔다.

## 예제 / 다이어그램 (선택)
```text
EDR Playbook (예)
1. 경보 발생: Suspicious PowerShell -> MITRE T1059 매핑
2. 자동 조치: 호스트 네트워크 격리, 프로세스 종료
3. 분석가 확인 후 IOC 피드백: SHA256 해시, C2 도메인 → TI 플랫폼에 등록
4. Sentinel/Splunk 알림을 통해 사건 관리 시스템(Jira/ServiceNow)과 연동
```
- `drawio/edr_ndr_lifecycle.drawio`에 경보 수집 → 상관분석 → SOAR 대응 → TI 피드백 흐름을 다이어그램으로 정리했습니다.

## TODO / 후속 연구
- [x] 주요 상용 솔루션(Microsoft Defender, CrowdStrike) 기능 비교 표 추가.
- [x] 공격 시나리오별(랜섬웨어, Living-off-the-Land) 탐지 룰 예시 정리.
- [x] 자동화(플레이북) 구성 시 주의할 false positive 사례 수집.

## 참고 자료
- [MITRE ATT&CK – Enterprise Matrix](https://attack.mitre.org/) – 전술·기술 매핑 기준.
- [Gartner Market Guide for EDR](https://www.gartner.com/) – 제품 선정 체크포인트.
- [SANS DFIR Summit Talks](https://www.sans.org/cyber-security-events/) – 실무 조사 발표 자료.
