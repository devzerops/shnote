- 마지막 업데이트: 2025-09-24
- 상태: 초안

# ELAM (Early Launch Anti-Malware)

## 개요
- Windows 8 이후 커널 드라이버가 로드되기 직전에 실행되는 부팅 보호 메커니즘.
- 서명된 보안 소프트웨어가 미리 신뢰 목록을 제공하고, 악성 드라이버 로드를 차단.

## 핵심 개념 / 절차
- **부팅 순서**: ELAM 드라이버 → 커널 초기화 → 부팅 시작 → 다른 서드파티 드라이버 로드.
- **Classifications**: Good, Bad, Bad but required, Unknown. 정책에 따라 허용 여부 결정.
- **구성 요소**: ELAM 인증 드라이버, ELAM 정책 파일, 서명된 공급자.

## 실무 / 시험 포인트
- ELAM은 Secure Boot, VBS(가상화 기반 보안)과 함께 사용 시 효과 극대화.
- Windows Defender뿐 아니라 서드파티 EDR도 ELAM 공급자로 등록 가능.
- 잘못된 분류(Bad but required) 시 부팅 실패 가능 → 롤백 계획 필수.

## 예제 / 다이어그램 (선택)
```text
ELAM 구성 흐름 (요약)
1. 공급자 인증: Microsoft Authenticode 서명 필수
2. ELAM 드라이버 설치 및 정책 등록
3. 테스트 모드에서 부팅 → 사건 로그(Event ID 7000, 7001) 확인
4. 운영 환경 배포 전 UEFI/Secure Boot 설정 검증
```
- 부팅 체인 도식은 `drawio/elam_boot_chain.drawio`에 작성 예정.

## TODO / 후속 연구
- [ ] Event ID 매핑(ELAM 관련 7000/7001/305/308) 요약표 추가.
- [ ] 공급자별 정책 템플릿(Microsoft, CrowdStrike 등) 비교.
- [ ] UEFI Secure Boot와의 통합 절차 작성.

## 참고 자료
- [Microsoft Docs – Early Launch Anti-Malware](https://learn.microsoft.com/windows/security/threat-protection/device-guard/early-launch-anti-malware)
- [Windows Event Logging Reference](https://learn.microsoft.com/windows/win32/wes/event-logging)
- [Secure Boot Overview](https://learn.microsoft.com/windows-hardware/design/device-experiences/oem-secure-boot)
