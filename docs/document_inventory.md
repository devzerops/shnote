# 문서 인벤토리

## TL;DR
- 상위 폴더 표에서 현재 상태와 추천 행동을 빠르게 확인할 수 있습니다.
- 우선 보강이 필요한 영역은 `basic_notes/`, `tools_notes/`, `tech/` 순입니다.
- 새로운 문서를 만들면 이 문서와 관련 README·FAQ 링크를 즉시 갱신하세요.

### 최근 업데이트 하이라이트 (2025-09-26)
- `basic_notes/algorithme/binary_search.md` 신규 작성: 이진 검색 전제 조건, Python 예제, 구현 주의 사항과 CLRS·MIT OCW 참고 자료를 포함했습니다.
- `basic_notes/network/communication/ARQ_PCM_Multiplexing.md`를 추가해 ARQ/PCM/가상회선/OFDMA/샤논 정리를 ITU-T·3GPP·RFC 자료와 함께 정리했습니다.
- `OS_notes/Scheduling_Deadlock_Thrashing.md`에서 스케줄링 비교, 교착 상태 대응, Thrashing 완화 전략을 Denning·Dijkstra·Red Hat 자료 기반으로 통합했습니다.
- `basic_notes/network/7_Layer/L3/protocol/OSPF.md`에 영역 설계 가이드, 코스트 튜닝 표, 트러블슈팅 체크리스트를 Cisco/Juniper 레퍼런스로 보강했습니다.
- `tech/VeraCrypt.md`, `tech/Bitlock.md`, `tech/DockerCompose.md`, `tools_notes/scan/nmap/logging.md`에 공식 문서 링크와 실무 로그 예시를 추가했습니다.
- `packet_detect.md`에 Zeek Ansible 배포 스니펫, Suricata CI/CD 룰 갱신 파이프라인, Arkime ↔ Security Onion 비교 표를 추가했습니다.

### 빠른 점프
- [상위 폴더 개요](#상위-폴더-개요)
- [상태 아이콘 힌트](#상태-아이콘-힌트)
- [미완성 또는 상태 미정 문서](#미완성-또는-상태-미정-문서)
- [정비 우선순위 제안](#정비-우선순위-제안)
- [분기 점검 체크리스트](#분기-점검-체크리스트)

## 상태 아이콘 힌트
- ✅ 완료: 현재 참고 가능. 개선 시 README/FAQ를 함께 갱신합니다.
- ⚙️ 진행 중: 보강 작업이 일부 진행되었으며 검증 대기 중입니다.
- ⏳ 예정: 초안이거나 내용이 부족합니다. 빠른 보강이 필요합니다.
- 📝 메모: 담당자/비고가 필요한 항목입니다. TODO에 세부 내용을 남겨 주세요.

## 상위 폴더 개요
| 폴더 | 주요 내용 | 상태 메모 | 사용자 권장 행동 |
|------|-----------|-----------|----------------|
| `basic_notes/` | 범용 지식 노트 | 통신/OSPF/이진 검색 노트 보강 완료, DB 문서 핵심 섹션 보강 필요 | 대표 문서의 TL;DR/TODO를 채우고 비어 있는 핵심 개념을 우선 보강하세요. |
| `dev_notes/` | 개발 일반 및 프로젝트 관리 | 파일별 분량 편차 큼, 일부 중복 가능 | 관심 있는 테마에서 중복·구식 내용을 정리하고, 적용 사례나 링크를 TODO에 기록하세요. |
| `drawio/` | 네트워크·보안 다이어그램 소스 | `.drawio` 전용, 메타데이터 없음 | 관련 문서에 다이어그램 사용처와 버전을 명시하고 필요 시 썸네일·요약을 추가하세요. |
| `file/` | 개별 문서/시험 자료 | 파일명 한글/자음 혼용, 분류 필요 | 필요한 자료를 정리하면서 명명 규칙 제안을 TODO에 남기고, 재분류가 필요하면 `docs/document_inventory.md`에 메모하세요. |
| `OS_notes/` | 운영체제 이론 정리 | ✅ process/thread 문서 통합 및 메타데이터 적용, 추가 문서 점검 필요 | 통합된 문서의 예제와 최근 커널 이슈를 보강하고 TL;DR 유무를 확인하세요. |
| `security_note/` | 보안 기술·솔루션 노트 | NAC 운영 가이드 보강 완료, EDR/NDR 실무 로그 보강 예정 | 최신 위협 레퍼런스를 추가하고 공격-대응 매핑이 누락되지 않았는지 점검하세요. |
| `tech/` | 기술·제품 실무 메모 | BitLocker 문서에 운영 체크리스트·다이어그램 추가, DD/ISO 문서 보강 필요 | 실무에서 확인한 설정값을 문서에 덧붙이고 draw.io TODO 진행도를 갱신하세요. |
| `tools_notes/` | 도구 사용법/단축키 | Docker 명령 문서에 점검 루틴·Compose 예제 추가, 기타 명령 예제 검증 필요 | 자주 쓰는 명령·옵션을 실제 결과와 비교 검증하고, TL;DR과 참고 링크를 보강하세요. |
| 루트 `.md` 파일 | 링크 모음 및 단일 노트 | `packet_detect.md` Zeek/Suricata 사례 추가, 일부 문서 최신성 미확인 | 단일 노트의 업데이트 날짜와 요약을 점검하고 FAQ/Q&A를 교차 링크하세요. |
| `docs/` | 메타 문서 및 작성 가이드 | `docs/getting_started.md`, `docs/writing_guidelines.md`, `docs/note_template.md`, `docs/faq.md` 유지 필요 | 템플릿·FAQ·안내 문서를 정기적으로 검토하고 변경 사항을 다른 문서로 즉시 전파하세요. |

## 중복·오탈자 파일
- `OS_notes/process.md`: ✅ `proccess.md` 통합 및 상태 전이 설명 보강.
- `OS_notes/thread.md`: ✅ `Tread.md` 통합, 스레드 모델 표준화.
- 기타 중복 여부는 미확인. 분기별 점검 필요.

## 미완성 또는 상태 미정 문서
- `security_note/quantum_cryptography_수정필요.md`: ✅ 1차 개정 완료, 사례 보강 예정.
- `today.md`: ✅ 네트워크·시스템 주제별로 재구성 완료.
- `check.md`: ✅ 체크리스트/백로그 테이블로 재정비.
- `packet_detect.md`: ⚙️ Zeek/Suricata 자동화·Elastic↔Grafana 파이프라인 문서화 완료, 최종 검토 및 다이어그램 PNG 삽입 대기.
- `basic_notes/` 내 다수 문서: ⏳ 개요/핵심 개념 섹션 채움 필요.
- `tech/` 전반: ⏳ draw.io 다이어그램 작성 및 비교 표 확장 예정.
- `tools_notes/` 기타 문서: ⏳ 명령 예제 검증·참고 자료 링크 추가 필요.
- `docs/getting_started.md`: ⚙️ 신규 사용자 피드백 반영 완료, 온보딩 영상/다이어그램 검토 대기.
- `docs/note_template.md`: ✅ 템플릿 초안 배포, 실제 문서 적용 사례 수집 예정.
- `security_note/defend/NAC.md`: ⚙️ Posture 체크리스트·자동화 예제 추가 완료, 게스트 포털 흐름 다이어그램 삽입 대기.

## 정비 우선순위 제안
1. `basic_notes/` 내 DB/수학 문서의 TL;DR·핵심 섹션을 보강하며, Oracle Concepts, PostgreSQL Manual 등 공식 문서를 교차 참조.
2. `tools_notes/` 주요 도구(`iptables`, `tcpdump`, `grep` 등)에 실제 실행 로그·에러 사례를 추가하고, Netfilter.org·tcpdump.org·GNU grep 매뉴얼 링크를 첨부.
3. `tech/DD.md`, `tech/ISO.md`에 복구/배포 로그와 draw.io 다이어그램을 작성한 뒤 문서에 링크하며, Rufus FAQ·ECMA-119·ISO 9660 표준 문서를 인용.
4. `packet_detect.md` Elastic ↔ Grafana 다이어그램을 PNG로 내보내 본문에 삽입하고, 관련 로그 예시를 추가 검토합니다.
5. `basic_notes/`, `tech/`, `tools_notes/`, `security_note/` README의 대표 링크 표와 TODO 상태를 월 1회 검토하고, 문서별 외부 레퍼런스 최신성을 확인.
6. NAC·BitLocker·OFDMA 다이어그램을 PNG로 내보내 문서 본문에 삽입하고 버전 정보를 기록하며, Cisco SRND·Microsoft Learn·3GPP TS 36/38 문서를 참조.

> **TIP**: 각 항목을 진행한 뒤에는 해당 폴더 README의 TODO 체크박스를 업데이트하고 커밋 메시지에 완료 사실을 남기면 추적이 쉽습니다.

## 유지관리 메모
- 작성 가이드 준수 여부는 `docs/writing_guidelines.md` 참고.
- 정비 완료 문서는 README 혹은 인덱스에서 직접 링크를 제공할 것.

## 분기 점검 체크리스트
- [ ] `docs/getting_started.md` 단계별 안내 최신화 및 상태 업데이트.
- [ ] `README.md` “빠르게 시작하기” 섹션의 폴더 경로·명칭 검증.
- [ ] `docs/writing_guidelines.md` 항목 점검 및 예시 최신화.
- [ ] `docs/note_template.md` 항목 검토 및 개정 필요 여부 확인.
- [ ] `basic_notes/README.md`, `tech/README.md`, `tools_notes/README.md` 대표 링크 표 상태·업데이트 검토.
- [ ] `security_note/README.md` 대표 링크 표 상태·업데이트 검토.
- [ ] `security_note/attack/README.md`, `security_note/defend/README.md`의 전술·대응 링크 최신화.
- [ ] 우선순위 테이블의 완료 여부 확인 후 필요시 리프레이징.
- [ ] 신규/삭제 문서 발생 시 상위 폴더 개요 테이블 갱신.
