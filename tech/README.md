- 마지막 업데이트: 2025-09-26
- 상태: 진행중

# 개요
`tech/` 폴더는 특정 제품·기술(예: BitLocker, ISO/IMG 작성, 디스크 모드)의 실무 관점 팁을 모았습니다. `basic_notes/`가 개념 중심이라면, 여기서는 운영 경험과 설정 사례를 기록합니다.

## TL;DR
- 운영자라면 `현재 문서 요약` 표에서 상태/업데이트를 확인하고 필요한 문서를 바로 엽니다.
- 실무 세팅은 각 문서의 `실무/시험 포인트`와 TODO 체크리스트에서 최신 정보를 먼저 확인하세요.
- 비슷한 주제가 `basic_notes/`와 겹치면, 여기서는 "어떻게"에 초점을 맞추고 개념 정리는 `basic_notes/`로 링크합니다.
- BitLocker/VeraCrypt/Docker Compose 문서에는 표준 로그 캡처 예시를 추가했으니, 현장 출력과 비교해 편차를 점검하세요.

### 빠른 점프
- [현재 문서 요약](#현재-문서-요약)
- [BitLocker vs VeraCrypt vs LUKS 비교](#bitlocker-vs-veracrypt-vs-luks-비교)
- [첫 10분 가이드](#첫-10분-가이드)
- [정비 메모](#정비-메모)
- [TODO / 후속 연구](#todo--후속-연구)
- [신규 VeraCrypt CLI 가이드](VeraCrypt.md)

## 현재 문서 요약
| 문서 | 상태/업데이트 | 주제 | 핵심 내용 | 링크 |
|------|---------------|------|-----------|------|
| `Bitlock.md` | 진행중 · 2025-09-26 | BitLocker 디스크 암호화 | TPM+PIN 운영 체크리스트, 자동화 스크립트 | [MS Learn BitLocker](https://learn.microsoft.com/windows/security/information-protection/bitlocker/) |
| `VeraCrypt.md` | 초안 · 2025-09-26 | 오픈소스 볼륨 암호화 | CLI 생성·마운트 절차, 감사 로그 포인트 | [VeraCrypt Docs](https://www.veracrypt.fr/en/Documentation.html) |
| `DockerCompose.md` | 초안 · 2025-09-26 | Compose 기반 오케스트레이션 | 실행/로그 수집 템플릿, 헬스체크 팁 | [Docker Compose Docs](https://docs.docker.com/compose/) |
| `DD.md` | 검토중 · 2025-09-24 | Rufus DD 모드 | 섹터 복제, 호환성, 복구 절차 | [Rufus FAQ](https://rufus.ie/en/#faq) |
| `ISO.md` | 검토중 · 2025-09-24 | ISO 이미지 포맷 | ISO 9660 구조, 부트 로딩 | [ECMA-119](https://www.ecma-international.org/publications-and-standards/standards/ecma-119/) |

> 표의 상태/업데이트 정보는 각 문서 상단 메타데이터를 기준으로 합니다.

## 첫 10분 가이드
1. `현재 문서 요약`에서 오늘 필요한 기술을 고릅니다.
2. 문서 상단 메타데이터와 TL;DR을 읽고, TODO에 남겨진 실무 확인 항목을 먼저 처리합니다.
3. 설정값이 궁금하면 `drawio/` 다이어그램 초안(예: `drawio/edr_ndr_lifecycle.drawio`)을 열어 흐름을 확인하고 필요한 경우 스크린샷을 추가합니다.

> **TIP**: 배포 전 검증 커맨드는 `tools_notes/`에 있는 도구별 노트와 교차 링크해 실행 로그를 남기세요.

## BitLocker vs VeraCrypt vs LUKS 비교
| 항목 | BitLocker | VeraCrypt | LUKS (dm-crypt) |
|------|-----------|-----------|-----------------|
| 지원 OS | Windows (Pro 이상) | Windows/Linux/macOS | Linux (커널 통합) |
| 관리 포인트 | AD/Azure AD 통합, GPO | GUI/CLI, 포터블 컨테이너 | CLI(`cryptsetup`), 배포 자동화 |
| 복구 키 | 자동 백업, 네트워크 언락 지원 | 수동 백업, PIM 설정 | 키 슬롯 다중 관리 |
| 하드웨어 요구 | TPM 권장 | 별도 요구 없음 | 별도 요구 없음 |
| 감사/로그 | Windows Event Log | 제한적 | syslog/`journalctl` |
| 사용 사례 | 엔터프라이즈 단말 | 개인/이식 가능한 볼륨 | 서버/리눅스 워크스테이션 |

> TODO: `drawio/bitlocker_vs_veracrypt.drawio`에 인증 흐름 다이어그램 초안을 작성하고, 문서에서 이미지 경로를 연결할 계획입니다.

## 정비 메모
- ✅ 문서별 템플릿 적용(2025-09-24).
- ✅ BitLocker/VeraCrypt CLI 체크리스트 정리(2025-09-26).
- ⏳ BitLocker/ISO/IMG 관련 실제 설정 캡처와 draw.io 도식 추가 예정.
- ⏳ 운영 사례(배포 실패, 복구 절차) 섹션 신설 필요.

# 핵심 개념
- 기술별 장단점을 비교해 같은 주제를 `basic_notes/`와 중복 기록하지 않도록 한다.
- 운영 경험은 재현 가능한 단계와 검증 결과를 포함해야 유용하다.

# 실무/시험 포인트
- 기업 단말 암호화 정책 비교(BitLocker vs LUKS)를 면접/시험에서 자주 묻기 때문에 기본 구분을 숙지한다.
- Rufus DD/ISO 모드 선택은 장애 대응 시 유용하므로 설치 실패 사례를 별도 문서화한다.

# TODO / 후속 연구
- BitLocker 네트워크 언락, LUKS 키 슬롯 관리 실습 캡처 추가.
- ISO/DD 비교 다이어그램을 `drawio/iso_vs_dd.drawio`로 작성하고 문서에 링크.
- 각 문서별 사례 연구(실제 운영 문제와 해결책) 섹션 신설.
- VeraCrypt 자동 마운트 스크립트에 대한 크로스-플랫폼 검증 노트 추가.
- [ ] `Bitlock.md`에 GPO 배포 절차 스크린샷 추가 (2025-Q4)
- [ ] `DD.md` 실습 로그(`dd --progress`)와 실패 복구 시나리오 정리
- [ ] `ISO.md`에 UEFI/Legacy 부트 비교표 삽입 및 참고 링크 확장

# 참고 자료
- Microsoft Learn – BitLocker Deployment Guides.
- VeraCrypt Documentation – User Guide.
- Red Hat Customer Portal – dm-crypt/LUKS Implementation Guide.
