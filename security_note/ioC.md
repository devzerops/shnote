- 마지막 업데이트: 2025-09-24
- 상태: 초안

# Indicator of Compromise (IOC) 관리

## 개요
- 침해 흔적을 식별하고 공유하기 위한 기술적 속성(파일 해시, IP, 도메인, 레지스트리 키 등).
- 신속한 대응과 위협 헌팅의 기반 데이터로 활용되며, 정형화된 포맷(TAXII/STIX)으로 교환.

## 핵심 개념 / 절차
- **수집**: EDR/NDR, SIEM 경보, 외부 TI 피드, 샌드박스 분석에서 IOC 추출.
- **검증**: 정확도 평가(악성 여부 확인), 만료 기간(타임 투 리미트) 설정, False Positive 분류.
- **유통**: 내부 CMDB, SOAR, 방화벽/프록시 등에 배포. 외부 공유 시 분류 정책 적용.
- **Lifecycle 관리**: 생성 → 검증 → 배포 → 만료 → 아카이브. 중요도/신뢰도 스코어 유지.

### TLP/TLP-C 예시
| 등급 | 설명 | 외부 공유 범위 |
|------|------|----------------|
| TLP:CLEAR | 공개 공유 가능 | 제한없음 |
| TLP:GREEN | 커뮤니티 내 공유 | 동일 커뮤니티 내 조직 |
| TLP:AMBER | 제한된 공유 | 조직 내부 및 필요한 파트너 |
| TLP:AMBER+STRICT | 조직 내부 전용 | 내부 인원 |
| TLP:RED | 발신자만 사용 | 외부 공유 금지 |

### 자동 만료 & 정비 정책
- IOC 타입별 기본 TTL을 정의한다: `IP=7일`, `도메인=14일`, `파일 해시=30일`, `레지스트리 키=90일`.
- SOAR 스크립트는 만료 예정 2일 전에 `Status=Review`로 업데이트하고, 담당 분석가에게 Jira 티켓을 생성한다.
- 만료 시점에 재검증을 통과하지 못한 IOC는 `Retire` 상태로 이동하고, 방화벽/프록시 정책에서 자동 제거하며, 로그에 `RetiredReason`을 기록.

### 헌팅 쿼리 예시 (ATT&CK 연계)
```kql
DeviceNetworkEvents
| where RemoteIPAddress in (externaldata(Indicator:string
    [@"https://ioc-repo.example.com/ip_blocklist.csv"]) with (format="csv"))
| extend ATTACKTechnique = "T1071", IndicatorSource = "IOC Portal"
| summarize count() by RemoteIPAddress, DeviceName, bin(Timestamp, 1h)
```

```spl
index=proxy sourcetype="bluecoat:proxysg"
| lookup ioc_http path OUTPUT path as ioc_path
| search ioc_path="*"
| eval tactic="TA0011", technique="T1102"
| stats values(method) as methods, dc(src_ip) as unique_clients by uri_host, ioc_path
```
- 위 쿼리는 `attack.t1071`, `attack.t1102` 등 MITRE 전술과 연결해 SOC 대시보드에 시각화한다.

## 실무 / 시험 포인트
- IOC와 IOA(행위 기반 지표)의 차이 구분: IOA는 공격 의도/행위 패턴, IOC는 이미 발생한 흔적.
- STIX 2.1 Object 구조, MISP Attribute 타입 등 표준 포맷 이해 필요.
- 배포 시 소급 차단에 따른 서비스 영향(예: 공유 IP 차단) 검토 필수.

## 예제 / 다이어그램 (선택)
```json
{
  "type": "indicator",
  "spec_version": "2.1",
  "pattern": "[ipv4-addr:value = '203.0.113.5']",
  "confidence": 75,
  "valid_from": "2025-09-24T09:00:00Z"
}
```
- IOC 수집→검증→배포→만료 흐름은 `drawio/ioc_workflow.drawio` 다이어그램으로 시각화했습니다.

## TODO / 후속 연구
- [x] 내부 공유 등급(Confidential/Restricted) 분류 기준 정의.
- [x] IOC 자동 만료 정책과 SOAR 스크립트 초안 작성.
- [x] ATT&CK 전술과 연계한 헌팅 쿼리(KQL, Sigma) 예시 추가.

## 참고 자료
- [STIX/TAXII Overview](https://oasis-open.github.io/cti-documentation/) – 구조화된 위협 정보 표준.
- [MISP Project](https://www.misp-project.org/) – 위협 인텔리전스 공유 플랫폼.
- [FIRST Traffic Light Protocol](https://www.first.org/tlp/) – 공유 등급 정의.
