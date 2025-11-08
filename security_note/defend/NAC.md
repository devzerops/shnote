- 마지막 업데이트: 2025-09-25
- 상태: 초안

# NAC(Network Access Control) 운영 가이드

## TL;DR
- 802.1X/게스트/격리 3단계 흐름을 `drawio/nac_access_flow.drawio`로 함께 검토하면 정책 설계가 빨라집니다.
- Posture 점검 결과에 따라 VLAN/ACL을 동적으로 바꾸는 자동화(예: pxGrid → DNA Center)부터 설계하세요.
- 장애 대비를 위해 fallback 인증(MAB, Guest)과 Remediation VLAN 정책을 미리 시뮬레이션합니다.

```mermaid
flowchart LR
    Start[장치 연결] --> Auth{802.1X 성공?}
    Auth -- 예 --> Posture[Posture 평가]
    Auth -- 아니오 --> Fallback[MAB 또는 게스트 포털]
    Posture --> Decision{정책 준수?}
    Decision -- 예 --> Prod[VLAN/ACL 허용]
    Decision -- 아니오 --> Remediation[Remediation VLAN / 제한 ACL]
    Remediation --> ITSM[SIEM/ITSM 알림]
    Fallback --> Guest[제한 Guest 네트워크]
    Prod --> Heartbeat[지속 평가 (Continuous Validation)]
    Guest --> Monitor[트래픽 모니터링]
```

## 개요
- 엔드포인트의 보안 상태를 검증한 뒤 네트워크 접근을 허용/차단하는 정책 기반 제어 체계.
- 802.1X, MAB(MAC Authentication Bypass), 게스트 포털을 활용해 인증·인가를 수행하고, 정책 미준수 장비를 격리한다.

## 요약 흐름
1. 장치 연결 → 1차 인증(802.1X/MAB) 시도.
2. Posture Agent 혹은 API로 컴플라이언스 점검.
3. 정책 매칭 결과에 따라 VLAN·ACL·SGT를 즉시 적용.
4. 미준수 장비는 Remediation VLAN으로 분리하고 ITSM/SIEM으로 알림 전파.

> 상세 단계는 `drawio/nac_access_flow.drawio`의 "NAC Decision Tree"를 참고하세요.

## 핵심 개념 / 절차
- **등록(Onboarding)**: 인증서/자격 증명을 배포하고, 에이전트 또는 에이전트리스 점검 방식을 정의.
- **검증(Posture Assessment)**: OS 패치, AV 상태, 디스크 암호화 등 컴플라이언스 상태를 평가.
- **정책 적용**: VLAN 할당, ACL 동적 푸시, SDN 연동을 통해 권한을 강제.
- **위협 대응**: 비인가 장치 탐지 시 격리 VLAN 또는 게스트 네트워크로 이동, 알림 생성.

### 솔루션 비교
| 항목 | Cisco ISE | Aruba ClearPass | Fortinet FortiNAC |
|------|-----------|-----------------|-------------------|
| 인증 방식 | 802.1X, MAB, WebAuth | 802.1X, MAB, Captive Portal | 802.1X, MAB, SNMP 제어 |
| 통합 | Cisco DNA Center, pxGrid | Aruba Central, MDM 연동 | FortiGate, FortiAnalyzer |
| 장점 | 대규모 엔터프라이즈, 풍부한 생태계 | BYOD/게스트 관리 용이, 유연한 정책 | 보안 패브릭 통합, 에이전트리스 탐지 |
| 고려사항 | 라이선스 복잡, 구축 비용 | 정책 설계 복잡성, 라이선스 | 네트워크 장비 호환성 체크 |

### Posture 체크리스트 (예시)
| 항목 | 점검 방법 | 목표 상태 | 자동화 도구 |
|------|-----------|-----------|-------------|
| OS 패치 레벨 | SCCM/Intune API 질의 | 최신 Cumulative Update 적용 | Microsoft Intune, Tanium |
| 엔드포인트 보안 | EDR API 상태 확인 | 실시간 보호 활성, 최신 엔진 | Defender for Endpoint, CrowdStrike |
| 디스크 암호화 | `manage-bde -status` / MDM 보고 | BitLocker/ FileVault ON | Intune BitLocker 리포트 |
| USB 제어 정책 | 레지스트리/MDM 정책 확인 | 승인된 장치만 허용 | AirWatch, Jamf |
| 네트워크 프로필 | NAC Agent 보고 | 회사 네트워크로 분류 | AnyConnect, ClearPass OnGuard |

### SDN/보안 장비 연동 사례
- **Cisco ISE + DNA Center/ACI**: pxGrid 이벤트를 통해 공격 의심 단말을 SD-Access Fabric에서 격리.
- **Aruba ClearPass + NSX-T**: Enforcement Profile이 NSX 정책에 태그를 추가해 마이크로세그멘테이션 적용.
- **FortiNAC + FortiGate**: Threat feed와 연동해 NAC 이벤트 발생 시 방화벽 정책을 동적으로 업데이트.

### 단계별 정책 예제 (Cisco ISE)
```bash
policy-set "Corp-Access"
  condition "Wired_8021X"
  authentication-rule 10
    if Radius:Service-Type == Framed AND NetworkAccess:EapAuthentication EQUALS EAP-TLS
    then use "Corp-Cert-Auth"
  authorization-rule 20
    if Posture-Status == Compliant
    then permit VLAN "VLAN_PROD" DACL "PROD_FULL_ACCESS"
  authorization-rule 30
    if Posture-Status == NonCompliant
    then permit VLAN "VLAN_REMEDIATION" DACL "LIMITED_ACCESS" and trigger CoA
  authorization-rule 40
    if EAP-Result == Failed
    then continue to MAB Fallback
```

### 자동화 Playbook 예제 (pxGrid → DNA Center)
```yaml
name: quarantine_non_compliant
triggers:
  - pxgrid_topic: com.cisco.ise.posture
    filter: postureStatus == "NonCompliant"
actions:
  - name: assign_remediation_vlan
    type: dnac_api
    endpoint: /dna/intent/api/v1/business/SDADevice?action=applyPolicy
    body:
      hostIp: "{{ endpoint.ip }}"
      policyName: "Remediation_VLAN"
  - name: create_incident
    type: itsm
    body:
      short_description: "NAC quarantine - {{ endpoint.mac }}"
      severity: 2
```

## 실무 / 시험 포인트
- 802.1X 인증 실패 시 fallback(MAB, Guest) 시나리오를 정의하지 않으면 서비스 장애 발생.
- NAC는 EDR/NDR 경보를 받아 자동으로 포트 차단 또는 VLAN 이동을 수행할 수 있다.
- 제로 트러스트 접근에서는 지속적인 재평가(Continuous Validation)를 통해 세션 중에도 상태를 감시한다.

### 게스트 포털 인증 흐름
1. 게스트 사용자가 SSID/포털 접속 → Captive Portal 페이지 표시.
2. 등록된 스폰서 또는 자동 승인 워크플로를 통해 임시 계정/OTP 발급.
3. 인증 성공 시 제한 VLAN(인터넷 전용)으로 이동하고, 세션 타이머(예: 8시간)를 적용.
4. NAC는 게스트 트래픽을 프록시/방화벽 로그와 연계해 이상 활동을 모니터링하고, 만료 시 자동 로그아웃 처리.

## 예제 / 다이어그램 (선택)
```text
1. 사용자가 유선 포트 연결 → 802.1X 인증 요청
2. RADIUS (ISE/ClearPass)에서 자격 검증 → Posture Agent 검사
3. 정책 일치 시 Prod VLAN, 불일치 시 Remediation VLAN 할당
4. NAC → SIEM 및 ITSM 티켓 발행
```
- NAC 인증부터 정책 적용, 격리까지의 절차는 `drawio/nac_access_flow.drawio`에서 확인할 수 있습니다.
- `drawio/ioc_workflow.drawio`와 연계하면 EDR·SIEM 이벤트와의 상관관계를 한 번에 시각화할 수 있습니다.

## TODO / 후속 연구
- [x] Posture 검증 항목(패치, AV, BitLocker) 체크리스트 표 작성.
- [x] SDN 기반(Nuage, ACI) NAC 연동 사례 수집.
- [x] 게스트 포털 인증 흐름 및 Captive Portal 사용자 경험 정리.

## 참고 자료
- [Cisco ISE Design Guide](https://www.cisco.com/) – 대규모 배포 모범 사례.
- [Aruba ClearPass Policy Manager](https://www.arubanetworks.com/) – BYOD 정책 템플릿.
- [Fortinet NAC Solution Brief](https://www.fortinet.com/) – 보안 패브릭 통합.
