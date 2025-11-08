- 마지막 업데이트: 2025-09-24
- 상태: 초안 (내용 보강 필요)

# Network Detection & Response

## 개요
- 네트워크 트래픽을 패시브/액티브로 모니터링하여 위협을 탐지하고 대응 자동화를 지원하는 솔루션.
- 기존 NIDS를 확장해 행동 분석, 지능형 탐지, 대응 플로우를 통합.

## 핵심 개념 / 절차
- **데이터 소스**: SPAN/TAP 트래픽, NetFlow/IPFIX, PCAP, TLS 핸드셰이크 메타데이터.
- **분석 기법**: 시그니처 기반(패턴 매칭) + 이상 탐지(통계, ML) + 위협 인텔 조합.
- **탐지 결과 처리**: 경보 → MITRE ATT&CK 네트워크 전술 매핑 → 티켓 발행/SOAR 연동.
- **대응 액션**: 방화벽/스위치 블록, NAC 격리, SOAR를 통한 자동 스크립트 실행.

### 솔루션 비교
| 항목 | Darktrace DETECT/RESPOND | Vectra AI Cognito | Zeek + Suricata (오픈소스) |
|------|-------------------------|-------------------|---------------------------|
| 배포 | 센서 어플라이언스 + SaaS 분석 | 온프렘/클라우드 센서 + SaaS | 자체 하드웨어/VM + OSS 구성 |
| 탐지 | AI 학습(자체 모델) + 시그니처 | AI 기반 행동 분석 + 시그니처 | 시그니처 + 스크립팅(Zeek) |
| 대응 | 자동 수동 격리, NAC 통합 | SOAR/방화벽 연계, 캠페인 뷰 | 외부 스크립트/자동화 필요 |
| 장점 | 설정 용이, 비정상 행위 탐지 강조 | Microsoft/O365 통합, 클라우드 가시성 | 커스터마이즈 자유도, 비용 절감 |
| 고려사항 | Black-box 모델, 비용 | Sensor 배치 설계 필요 | 운영 인력·튜닝 노력 필요 |

## 실무 / 시험 포인트
- EDR과 비교 시 네트워크 중심(에이전트 비필요)이라 IoT/OT 환경에서도 활용 가능.
- 암호화 트래픽 증가로 TLS 핑거프린트, JA3/JA3S 해시, SNI 분석이 중요해짐.
- 배치 고려사항: 고속 링크 처리(>10Gbps) 위해 하드웨어 Bypass, 패킷 드롭률 측정 필요.

## 예제 / 다이어그램 (선택)
```text
NDR Investigation 흐름 예시
1. Beaconing 이상 탐지 (DNS over HTTPS 주기적 요청)
2. 자동 태그: Suspicious Command-and-Control
3. SOAR -> 방화벽 정책 업데이트, Client VLAN 격리
4. 결과 요약을 SIEM에 전송, 사건번호와 연계
```
- `drawio/edr_ndr_lifecycle.drawio` 다이어그램에서 EDR 경보와 NDR 상관분석 후 SOAR 조치까지의 상호 흐름을 확인할 수 있습니다.

### JA3/JA3S 핑거프린트 수집 예시
```bash
# Zeek ja3 스크립트 로드 후 로그 예시 (zeek.log)
1521911723.655939	C8YcFX2S3bh	192.0.2.10	443	198.51.100.5	51584	TLS	JA3	74c1e... 	ja3_hash=72a589da586844d7f0818ce6849492a3
```
- Suricata는 `eve.json`에서 `tls.ja3.hash` 필드를 제공하며, NDR은 이를 기반으로 비정상 세션을 태깅한다.
- 공격자는 JA3 우회를 위해 TLS 라이브러리 스푸핑, DoH/ESNI 사용, Traffic padding 등을 활용하므로 예외 처리 정책을 함께 정의한다.

### 오픈소스 NDR 구축 절차(Zeek + Suricata)
1. **센서 노드 준비**: 고속 NIC와 충분한 디스크(IOPS)를 갖춘 Linux 노드를 구성하고, `AF_PACKET` 또는 `PF_RING` 기반 패킷 캡처 설정.
2. **패킷 분기**: SPAN/TAP 포트를 통해 미러 트래픽을 수집, `ethtool -K <iface> gro off lro off`로 오프로드 비활성화.
3. **Suricata 설치**: 최신 룰셋(Emerging Threats 등)을 적용하고 `eve.json` 출력에서 `flow`, `http`, `tls`, `dns`, `alert` 섹션 활성화.
4. **Zeek 설치**: `zeekctl`로 클러스터 구성 후 `ja3`, `intel`, `known-services` 스크립트를 로드하여 가시성 확보.
5. **로그 통합**: Suricata `eve.json`과 Zeek 로그를 Logstash/Fluentd로 수집해 OpenSearch/Splunk에 적재, MITRE 전술 태그를 추가.
6. **상호 연계**: Suricata 경보를 Zeek Intelligence Framework로 푸시해 추후 세션에서 동일 IOC를 빠르게 탐지하고, SOAR로 전달.

### Response Latency 측정 계획
- 경보 발생부터 SOAR 조치 완료까지의 SLA를 3단계로 측정한다: `Detection_latency`, `Enrichment_latency`, `Action_latency`.
- SOAR 플레이북에 타임스탬프 로깅을 삽입하고, 주간 리포트에서 평균·상위 95퍼센타일을 산출.
- 30초 이상 지연되는 케이스는 NAC/방화벽 API 응답, 인증 시스템 호출, 승인 워크플로 병목으로 분류하여 개선 작업을 진행한다.

## TODO / 후속 연구
- [x] JA3/JA3S 핑거프린트 수집 템플릿 및 우회 사례 정리.
- [x] Suricata/Zeek 기반 오픈소스 NDR 구축 절차 작성.
- [x] SOAR 연계 시 Response latency 지표 수집 계획 수립.

## 참고 자료
- [Gartner NDR Market Guide](https://www.gartner.com/) – 주요 벤더 기능 비교.
- [Zeek Documentation](https://docs.zeek.org/) – 오픈소스 네트워크 가시성 도구.
- [Suricata User Guide](https://docs.suricata.io/) – 시그니처 기반 탐지와 로깅.
