# 지식 베이스 허브
이 저장소는 보안·네트워크·운영체제·개발 관련 학습 자료와 참고 링크를 정리한 개인 노트입니다. 아래 표로 전체 구조를 먼저 확인한 뒤, 관심 있는 주제의 디렉터리나 문서를 탐색하세요.

## TL;DR
- 루트 → 폴더별 `README` → 세부 문서 순으로 보면 길을 잃지 않습니다.
- 보안 빠른 열람: `security_note/README.md`로 전술·대응 매핑부터 확인
- 네트워크/시스템 개요: `OS_notes/`, `tech/README.md`에서 필요한 문서를 찾기
- 개발/관리 노트: `basic_notes/README.md`, `dev_notes/`의 주제별 문서부터 읽기
- 도구 치트시트: `tools_notes/`에서 CLI·분석 도구 명령 예제 확인

## 구조 한눈에 보기
| 카테고리 | 경로 | 내용 요약 |
|----------|------|-----------|
| 보안 | `security_note/` (`security_note/README.md`), `owasp/`, `packet_detect.md` | 기술·솔루션 요약, 위협 인텔, 취약점 참고 자료 |
| 네트워크/시스템 | `OS_notes/`, `tech/` (`tech/README.md`), `drawio/` | 운영체제 이론, 네트워크 프로토콜, 다이어그램 |
| 개발 일반 | `dev_notes/`, `basic_notes/` (`basic_notes/README.md`) | SW 공정, 아키텍처, 개발 개념 정리 |
| 실습 & 체크리스트 | `today.md`, `check.md` | 현재 정리 중인 학습 과제와 TODO |
| 도구 사용법 | `tools_notes/` | CLI/시스템 도구 팁과 키 바인딩 |
| 기타 자료 | `file/`, `packet_detect.md` | 시험·문서 스크랩, 단일 참고 메모 |
| 메타 | `docs/getting_started.md`, `docs/document_inventory.md` | 신규 사용자 안내, 문서 상태·중복·미완성 항목 인벤토리 |

## 빠르게 시작하기
- 처음 방문했다면 `docs/getting_started.md`를 열어 문서 메타데이터(상태·최종 업데이트)와 탐색 순서를 익힙니다.
- 빠르게 흐름을 잡고 싶다면 `docs/getting_started.md#첫-5분-큐레이션`에서 3단계 요약을 확인하세요.
- 필요한 주제를 찾을 때는 위 표에서 폴더를 고른 뒤, 해당 디렉터리의 `README` 또는 인덱스 문서를 먼저 확인하세요.
- 문서 상태가 `초안`이라면 `docs/document_inventory.md`에서 보강 계획을, `완료`라면 참고 링크와 실무 포인트를 우선 확인하면 됩니다.
- 키워드 검색은 루트에서 `rg <키워드>`를 실행하면 전체 노트를 빠르게 탐색할 수 있습니다.
- 새 문서를 추가할 때는 `docs/writing_guidelines.md`와 `docs/note_template.md`를 참고해 메타데이터와 섹션 구조를 맞추세요.
- 기존 문서를 갱신할 때는 TL;DR과 상태 정보를 최신화하고, 관련 README·FAQ 링크도 함께 점검하세요.
- 보안 전술/방어 흐름은 `security_note/README.md`와 하위 `attack/README.md`, `defend/README.md`에서 대표 문서를 먼저 확인하세요.
- 자주 묻는 질문은 `docs/faq.md`에서 빠르게 확인할 수 있습니다. 재방문 시 변경점은 `docs/faq.md#q9-최근-업데이트를-빨리-확인하려면`에서 추적하세요.

## 핵심 참고 링크 모음
### 보안 & 침해대응
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) – 웹 취약점 우선순위 파악
- [Exploit DB](https://exploit-db.com) – 공개 익스플로잇 데이터베이스
- [Netfilter 아키텍처](https://pr0gr4m.github.io/linux/kernel/netfilter/) – Linux 패킷 필터 흐름 이해
- [iptables DDoS 대응 가이드](https://javapipe.com/blog/iptables-ddos-protection/) – 기본적인 우회 및 완화 전략
- [이중 인증서 체계 사례](https://palant.info/2023/01/02/south-koreas-online-security-dead-end/) – 국내 금융권 보안 이슈 분석

### 네트워크 & 시스템
- [Cisco OSPF 가이드](https://www.cisco.com/c/ko_kr/support/docs/ip/open-shortest-path-first-ospf/7039-1.html)
- [Subnetting 가이드](https://www.comparitech.com/net-admin/subnetting-guide/) – 주소 설계 시 참고
- [Security Onion 사용자 그룹](https://groups.google.com/g/security-onion/c/KC5GlhM5RVw/discussion)

### 윈도우 / OSINT 자료
- [Active Directory 침투 마인드맵](https://tajdini.net/blog/penetration/active-directory-penetration-mind-map/)
- [Sysinternals Security Utilities](https://learn.microsoft.com/ko-kr/sysinternals/downloads/security-utilities)
- [Malware-Traffic Analysis](https://www.malware-traffic-analysis.net/) – 패킷 분석 실습 자료

### 개발 & 도구
- [Quicktype](https://app.quicktype.io/) – JSON ↔ 모델 변환 도구
- [Tables Generator](https://www.tablesgenerator.com/markdown_tables) – Markdown 표 작성
- [Offensive Security Cheatsheets](https://www.ired.team/offensive-security-experiments/offensive-security-cheetsheets)
- [Python Tutor](https://pythontutor.com/) – 코드 실행 흐름 시각화

## 진행 중 과제
- `docs/document_inventory.md`의 정비 우선순위 표를 확인하고 담당할 항목을 체크하세요.
- `packet_detect.md`에 Zeek/Suricata 최신 레퍼런스를 추가하고 요약을 갱신해 보세요.
- `basic_notes/`와 `tech/`의 대표 문서에서 미완성 TODO를 확인한 뒤 채워 넣어 주세요.

## 유지관리 메모
- 최신 문서 점검 현황은 `docs/document_inventory.md`에서 확인할 수 있습니다.
- 새 문서를 추가할 때는 파일명에 명확한 주제를 사용하고, 상단에 작성일과 상태(초안/검토완료)를 표기하세요.
- 마지막 업데이트: 2025-09-25 (온보딩 흐름 갱신, FAQ 교차 링크 추가)
