- 마지막 업데이트: 2025-09-26
- 상태: 검토중

# 네트워크 패킷 분석 도구 개요
침해 대응에서는 단일 솔루션이 아닌 다수의 패킷 분석/가시화 도구를 조합해 상황 인식과 탐지를 수행합니다. Zeek, Suricata, SIEM/대시보드 도구는 서로 다른 목적을 담당하며 상호 보완적으로 운용됩니다.

# 핵심 도구 비교
| 도구 | 유형 | 주요 역할 | 강점 | 유의 사항 |
|------|------|-----------|-------|------------|
| Zeek (구 Bro/Zeek) | 네트워크 보안 모니터링(NSM) 프레임워크 | 트래픽 세션 해석, 풍부한 로그/아티팩트 생성 | 프로토콜 파서와 스크립팅 확장성, 이상 징후 탐색에 강점 | 실시간 차단 불가, 로그 저장·분석 파이프라인 필요 |
| Suricata | IDS/IPS/NSM 엔진 | 시그니처 기반 탐지, 통합 IDS/IPS/NSM | 멀티스레딩 성능, 최신 위협 인텔 서명 지원 | 정책/시그니처 관리와 튜닝 필수 |
| Kibana | Elasticsearch 대시보드 | 로그 시각화·검색 | Elastic Stack과 긴밀 통합, 알림 플러그인 다양 | 백엔드 ES 클러스터 용량 계획 필요 |
| Grafana | 시계열/메트릭 시각화 | 대시보드, 경보, 데이터 소스 집약 | 다양한 데이터 소스(ES, Prometheus 등) 지원 | 로그 탐색은 Loki 등 추가 컴포넌트 요구 |

# 운영 포인트
- Zeek 로그는 `conn`, `http`, `dns` 등 세부 파일로 구분하므로, 파서·인제스터에서 스키마를 명확히 정의해야 합니다.
- Suricata는 `eve.json` 단일 포맷을 사용하므로 SIEM 연동이 상대적으로 단순하지만, 잡음 제거를 위해 룰셋 튜닝이 필요합니다.
- Kibana는 ElasticSearch 기반의 대화형 검색·필터링에 강하며, Grafana는 장기 트렌드 그래프와 경보 워크플로에 최적화되어 둘을 조합하기도 합니다.
- 대규모 환경에서는 패킷 캡처(PCAP) 저장소와 NSM 로그를 연동해 인시던트별 재생산(provenance)을 확보하는 것이 중요합니다.

## Zeek 스크립트 확장 사례
- `zkg`(Zeek Package Manager)를 사용하면 `intel`, `notice` 프레임워크를 확장하는 패키지를 신속히 배포할 수 있습니다.
  - [`corelight/zeek-community-id`](https://github.com/corelight/zeek-community-id) – Zeek `conn` 로그에 JA3/CommunityID 지표를 추가.
  - [`zeek/zeek-ja3`](https://github.com/zeek/zeek-ja3) – TLS 핑거프린트를 `notice` 이벤트로 생성.
  - [`initconf/zeek-intel`](https://github.com/initconf/zeek-intel) – OSINT 인텔 피드를 자동으로 `intel.log`에 반영.
- 패키지 설치 예시: `zkg refresh && zkg install zeek/zeek-ja3`.
- `notice` 정책 예: `@load policy/protocols/ssl/validate-certs`로 비정상 인증서 탐지 알림을 생성.

## Suricata 룰셋 유지보수 사례
- `suricata-update` 유틸리티로 Emerging Threats Open(ET Open), Proofpoint ET Pro, Abuse.ch 등 외부 룰 피드를 자동 갱신.
  - 기본 명령: `suricata-update --disable-conf=/etc/suricata/disable.conf && systemctl reload suricata`.
  - [Emerging Threats Open Ruleset](https://rules.emergingthreats.net/) – 최신 악성 도메인/IP/시그니처 제공.
  - [Abuse.ch SSLBL Suricata Rules](https://sslbl.abuse.ch/blacklist/) – 악성 TLS 핸드셰이크 탐지 룰.
- 룰 배포 전 `suricata -T -c /etc/suricata/suricata.yaml`로 구문 검증을 수행하고, `eve.json` 경고 값 분류 체계를 Kibana에서 시각화합니다.

## Zeek 스크립트 배포 자동화 예시
- `ZeekCtl`은 소규모 환경에서 빠르게 재시작/설정 반영을 처리하지만, 다수의 센서 노드를 운영할 때는 구성 관리 도구를 함께 사용하는 편이 효율적입니다.
- Ansible 기반 플레이북 워크플로 예시:
  1. `zkg refresh`로 패키지 인덱스를 동기화하고, `zkg install --force`로 필수 스크립트를 재배포합니다.
  2. Zeek 스크립트 디렉터리를 템플릿화(`roles/zeek/files/site/local.zeek.j2`)하고, 호스트 변수에 따라 `@load` 목록을 조정합니다.
  3. 변경 사항이 감지되면 `zeekctl deploy`를 실행해 정책·노드 구성을 동시에 반영합니다.
- 간단한 Ansible 태스크 스니펫:
```yaml
- name: Deploy Zeek packages
  ansible.builtin.shell: |
    zkg refresh
    zkg install --force {{ item }}
  loop: "{{ zeek_required_packages }}"
  args:
    chdir: /opt/zeek/share/zeek/site

- name: Render Zeek local policy
  ansible.builtin.template:
    src: local.zeek.j2
    dest: /opt/zeek/share/zeek/site/local.zeek
    mode: "0644"

- name: Deploy Zeek cluster config
  ansible.builtin.template:
    src: node.cfg.j2
    dest: /opt/zeek/etc/node.cfg
    mode: "0644"

- name: Apply Zeek configuration
  ansible.builtin.command: zeekctl deploy
  register: deploy_result
  changed_when: "changed" in deploy_result.stdout
```
- 테스트 팁: `zeekctl check`로 파서 오류를 조기에 발견하고, CI 파이프라인에서는 `zeek -Be <script>` 명령으로 스크립트 컴파일 테스트를 자동화합니다.

## Suricata 업데이트 파이프라인 자동화
- CI/CD 파이프라인에서 Suricata 룰 갱신을 표준화하면 신규 시그니처 적용 시 휴먼 에러를 줄일 수 있습니다.
- 표준 단계 예시:
  1. 스테이징 노드에서 `suricata-update --no-reload`로 룰셋을 내려받고 Git 저장소나 Artifactory에 보관합니다.
  2. PR 혹은 머지 시 `suricata-verify -l ./verify`로 탐지 케이스를 검증하며, 실패 시 파이프라인을 중단합니다.
  3. 운영 노드에는 검증된 룰셋을 배포한 뒤 `suricata -T -c /etc/suricata/suricata.yaml -S /etc/suricata/rules/suricata.rules` 명령으로 최종 검증을 수행합니다.
  4. Systemd 단위에서는 `ExecReload=/usr/bin/suricata-update && /usr/bin/systemctl kill -s USR2 suricata`처럼 소프트 리로드 신호(USR2)를 활용해 트래픽 드롭 없이 룰을 교체합니다.
- Jenkins/GitLab CI 예시:
```yaml
suricata-rules:
  stage: security
  image: oisf/suricata:7.0
  script:
    - suricata-update --no-reload --suricata-conf /tmp/suricata.yaml
    - suricata-verify -l verify/
    - tar czf artifacts/suricata-rules.tar.gz /var/lib/suricata/rules
  artifacts:
    paths:
      - artifacts/suricata-rules.tar.gz
      - verify/
```
- 알림: `eve.json`을 Filebeat/Vector로 전송할 경우, 룰 해시나 릴리스 노트를 필드에 추가해 추적 가능하도록 합니다.

## 통합 배포판 비교: Arkime vs Security Onion
| 항목 | Arkime(Moloch) | Security Onion |
|------|----------------|----------------|
| 핵심 기능 | 대용량 PCAP 인덱싱·검색, 세션 재구성 | Zeek/Suricata/Wazuh 등 통합 NSM 배포판 |
| 배포 난이도 | 독립형 설치, Elastic Stack 필요 | ISO 이미지·호스트형 배포 제공, 설정 마법사 포함 |
| 강점 | 빠른 패킷 재생산, SSO/LDAP 통합 지원 | 통합 대시보드·알림, 다양한 센서 역할 지원 |
| 보완 포인트 | 탐지 로직은 외부 IDS에 의존, 장기 저장소 설계 필요 | 리소스 요구량이 크며, 구성요소 업데이트 관리 필요 |
| 활용 전략 | 기존 SIEM과 연계해 증거 확보 · 포렌식에 집중 | 중소 규모 팀이 빠르게 NSM 스택을 구축할 때 적합 |
- 두 솔루션은 상호 보완적으로 사용할 수 있으며, Security Onion 센서에서 추출한 PCAP을 Arkime에 전송해 장기 보관하는 구성이 일반적입니다.
- Elastic ↔ Grafana 연계 다이어그램은 `drawio/elastic_grafana_flow.drawio`에서 확인할 수 있으며, 센서 → 수집 → 처리 → 저장 → 시각화 흐름을 한눈에 정리했습니다.

## Elastic ↔ Grafana 연동 개요
- **수집 계층**: Zeek와 Suricata는 각각 `conn.log`/`http.log`/`dns.log`, `eve.json`을 생성하며, Filebeat 또는 Logstash Forwarder가 TLS로 중앙 Logstash 노드에 전송합니다.
- **처리 계층(Logstash)**: 입력 플러그인은 `beats`/`file`을 사용하고, `grok`과 `geoip` 필터로 필수 필드를 정규화합니다. `mutate` 필터에서 `@timestamp`, `tags`, `rule_id` 등 공통 필드를 표준화해 시각화에 활용합니다.
- **저장 계층(Elasticsearch/OpenSearch)**: 인덱스는 하루 또는 주 단위(`zeek-YYYY.MM.DD`, `suricata-YYYY.MM.DD`)로 롤오버하며, 수명 주기 관리(ILM) 정책으로 핫-웜-콜드 저장소를 구분합니다.
- **시각화 계층**:
  - Kibana는 탐색/검색/타임라인 분석에 활용하고, 탐지 룰 튜닝 시 Dev Tools로 쿼리를 검증합니다.
  - Grafana는 장기 트렌드와 운영 지표(패킷 처리량, 경보 per rule_id)를 대시보드로 노출하며, Alerting으로 PagerDuty/Slack 알림을 트리거합니다.
- **데이터 파이프라인 베스트 프랙티스**:
  1. 인덱스 템플릿(`index template v2`)에서 필드 타입을 미리 정의해 매핑 충돌을 방지합니다.
  2. Zeek·Suricata 로그에 `sensor_id`, `enrichment_version` 필드를 추가해 룰 변경 시점을 추적합니다.
  3. Grafana는 Elasticsearch를 데이터 소스로 직접 조회하고, 필요 시 Loki/Prometheus 메트릭과 조합해 통합 대시보드를 구축합니다.
  4. 장기 보관은 S3/MinIO에 스냅샷을 생성하거나, Arkime과 연계해 PCAP ↔ 로그 상호 참조를 유지합니다.

# TODO / 후속 연구
- [x] Zeek 스크립트 배포 자동화(Ansible/ZeekCtl) 플레이북 샘플 작성.
- [x] `suricata-update` 자동화 파이프라인과 테스트 케이스(`suricata-verify`) 문서화.
- [x] Elastic Stack ↔ Grafana 연동 아키텍처 도식화 후 `drawio/`에 추가 (`drawio/elastic_grafana_flow.drawio`).
- [x] Moloch/Arkime, Security Onion 등 통합 배포판 비교 섹션 작성.

# 참고 자료
- [Zeek 공식 문서](https://docs.zeek.org/en/current/) – 스크립트 참조와 로그 설명.
- [Zeek Package Manager(zkg) 안내](https://docs.zeek.org/projects/package-manager/en/stable/) – 패키지 설치 및 관리.
- [Suricata User Guide](https://docs.suricata.io/) – 설치 및 룰셋 관리.
- [suricata-update 공식 문서](https://docs.suricata.io/en/latest/rule-management/suricata-update.html) – 룰 자동 업데이트.
- [Elastic Stack Observability](https://www.elastic.co/observability) – Kibana/Elasticsearch 연동 사례.
- [Grafana Alerting 문서](https://grafana.com/docs/grafana/latest/alerting/) – 대시보드 알림 설정.
