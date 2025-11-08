- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
`iptables`는 리눅스 커널의 Netfilter 프레임워크를 제어해 패킷을 허용/차단하거나 NAT를 구성하는 CLI 도구입니다. 체인(chain)과 테이블(table) 개념으로 규칙을 조직하며, 안전한 배포를 위해 테스트 환경에서 먼저 검증하는 것이 필수입니다.

## 기본 명령 형식
```bash
iptables [-t <table>] <action> <chain> [match 조건] -j <target>
```

# 핵심 개념
- **테이블**
  - `filter`: 기본 패킷 필터링.
  - `nat`: 주소/포트 변환(SNAT, DNAT, MASQUERADE).
  - `mangle`: ToS/TTL 등 패킷 수정.
  - `raw`: 연결 추적(conntrack) 제외 설정.
  - `security`: SELinux와 연동되는 MAC 정책 (RHEL 7+).
- **체인**
  - `INPUT`: 로컬 시스템에 도착하는 패킷.
  - `OUTPUT`: 로컬에서 나가는 패킷.
  - `FORWARD`: 라우팅되는 패킷.
  - `PREROUTING`/`POSTROUTING`: NAT 적용 전후 단계.
- **타깃(Target)**: `ACCEPT`, `DROP`, `REJECT`, `LOG`, `DNAT/SNAT`, 사용자 정의 체인 등.
- **상태 추적**: `-m state --state NEW,ESTABLISHED,RELATED` 등을 활용해 연결 상태별로 제어합니다.
- **정책**: `iptables -P INPUT DROP` 같이 기본 정책을 설정하고, 허용 규칙을 명시적으로 추가합니다.
- **성능 고려**: 규칙은 위에서부터 일치 여부를 검사하므로, 자주 매칭되는 허용 규칙을 상단에 배치하고 LOG 타깃은 최소화합니다.
- **IPv6**: `ip6tables` 명령이 별도로 존재하며, nftables로 전환 시 `iptables-nft` 호환 레이어를 확인합니다.

# 실무/시험 포인트
- 변경 전 `iptables-save > backup.rules`로 백업하고, 테스트 모드에서는 `service iptables save` 전 복구 경로를 마련합니다.
- SSH 차단을 방지하려면 `iptables -A INPUT -p tcp --dport 22 -j ACCEPT` 같은 허용 규칙을 먼저 추가한 뒤 기본 정책을 DROP으로 변경합니다.
- 로그 수집 시 `-j LOG --log-prefix "iptables_drop:"` 형태로 SIEM과 연계할 수 있습니다.
- 시험에서는 체인/테이블 역할, `-A`와 `-I` 차이, NAT 예제(SNAT, DNAT) 등을 자주 출제합니다.
- 커널 5.x 환경에서는 nftables가 기본이므로 중장기적으로 `nft` 기반 정책으로 이전하는 로드맵을 수립합니다.
- Kubernetes 등 컨테이너 환경에서는 kube-proxy가 iptables 규칙을 대량 생성하므로, `iptables-save | wc -l`로 규칙 수를 모니터링하고 성능 튜닝을 진행합니다.

# TODO / 후속 연구
- `iptables-save`/`iptables-restore`를 이용한 배포 자동화 예시 추가.
- nftables 전환 가이드와 명령 비교 표 작성.
- 일반적인 웹 서버 허용 정책 스크립트 예제 첨부.
- AWS/GCP 등 클라우드 환경에서 보안 그룹과 iptables 관계 정리.

# 참고 자료
- Netfilter/iptables 프로젝트 문서 – [https://netfilter.org](https://netfilter.org).
- Red Hat – *Using iptables to configure firewall rules*.
- `man iptables` – 옵션 및 모듈 설명.
- CNCF – *Kubernetes Network Policies* (iptables 백엔드).
