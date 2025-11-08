- 마지막 업데이트: 2025-09-25
- 상태: 검토중

# 개요
BitLocker(비트로커)는 Microsoft Windows에 내장된 전체 디스크 암호화 기능으로, TPM(Trusted Platform Module)과 복구 키를 활용해 저장 매체 분실 시 데이터를 보호합니다. Rufus 등으로 암호화된 USB를 구성할 때도 동일한 원리를 적용할 수 있습니다.

## TL;DR
- TPM+PIN 2요소를 기본값으로 두고, 복구 키는 Azure AD/AD DS에 자동 백업되도록 GPO에서 강제하세요.
- 배포 전 `manage-bde -status`, `Get-BitLockerVolume`로 암호화 진행률과 보호 상태를 스크립트로 검증합니다.
- 인증 흐름은 아래 Mermaid 순서도 또는 `drawio/bitlocker_flow.drawio`(TPM → PIN → OS Unlock)를 참고해 사용자 교육 자료로 활용할 수 있습니다.

```mermaid
flowchart LR
    TPM[TPM PCR 검증] --> PIN[사용자 PIN 입력]
    PIN --> Decision{검증 성공?}
    Decision -- 예 --> Unlock[FVEK 해독 및 OS 드라이브 언락]
    Unlock --> Boot[Windows 부팅 & 보호 상태 보고]
    Boot --> Logs[Event 24577/24588 로그 → SIEM]
    Decision -- 아니오 --> Recovery[복구 키 입력 (USB/AD/Azure AD)]
    Recovery --> Boot
```

# 핵심 개념
- **암호화 범위**: OS 파티션, 고정 데이터 드라이브, 이동식 드라이브(BitLocker To Go)까지 선택적으로 지원.
- **보안 구성요소**: TPM 2.0 이상 권장, PIN/패스워드/USB 키 조합으로 이중 인증 가능.
- **복구 메커니즘**: 복구 키는 Azure AD, Active Directory, 파일/프린터 출력 등 다중 경로에 백업 가능해야 함.
- **암호화 알고리즘**: XTS-AES 128/256비트, 구형 OS에서는 CBC 모드 지원.

## 배포 시나리오 예제
1. **정책 강제**: GPO에서 OS 드라이브 BitLocker 활성화 정책을 `Enabled`로 설정하고, `Require additional authentication` 옵션에서 TPM+PIN을 강제한다.
2. **스크립트 확인**:
   ```powershell
   # BitLocker 상태 요약
   Get-BitLockerVolume | Select-Object MountPoint,VolumeType,ProtectionStatus,EncryptionPercentage

   # 복구 키 백업 (AD DS)
   manage-bde -protectors -get C:
   manage-bde -protectors -adbackup C: -id {Protector-GUID}
   ```
3. **재부팅 테스트**: PIN 입력 후 부팅, `Event Viewer > Applications and Services Logs > Microsoft > Windows > BitLocker-API`에서 이벤트 ID 24577(정상 Unlock) 확인.
4. **복구 시뮬레이션**: 테스트 장비에서 복구 키를 사용해 잠금 해제, Audit 로그와 ITSM 티켓 기록.

> **TIP**: `Suspend-BitLocker -MountPoint C: -RebootCount 1` 명령으로 펌웨어 업데이트 시 일시적으로 보호를 중지할 수 있습니다.

## TPM+PIN 확인 스크립트 템플릿
아래 PowerShell 스니펫을 `C:\Scripts\Check-BitLocker.ps1`로 저장해 시작 전 점검에 활용합니다.

```powershell
param(
  [switch]$ExportCsv,
  [string]$OutputPath = "C:\\Temp\\bitlocker_status.csv"
)

$volumes = Get-BitLockerVolume | Select-Object `
  MountPoint,
  VolumeType,
  ProtectionStatus,
  EncryptionMethod,
  KeyProtector | ForEach-Object {
    [pscustomobject]@{
      MountPoint        = $_.MountPoint
      VolumeType        = $_.VolumeType
      ProtectionStatus  = $_.ProtectionStatus
      EncryptionMethod  = $_.EncryptionMethod
      HasTpmProtector   = ($_.KeyProtector | Where-Object KeyProtectorType -eq 'TpmPinProtector') -ne $null
      PercentageEncrypted = $_.EncryptionPercentage
    }
  }

if ($ExportCsv) {
  $volumes | Export-Csv -NoTypeInformation -Path $OutputPath
}

$volumes | Format-Table -AutoSize
```

- `HasTpmProtector` 값을 통해 TPM+PIN 적용 여부를 즉시 검증할 수 있습니다.
- CSV 로그는 Intune/Azure Monitor 업로드 전처리로 활용합니다.

## 운영 로그 수집 및 보고
- `manage-bde -status > C:\\Logs\\BitLocker-status-$(Get-Date -Format yyyyMMdd).txt`
- `Get-WinEvent -LogName "Microsoft-Windows-BitLocker-API/Operational" -MaxEvents 50 > C:\\Logs\\BitLocker-events.evtx`
- SIEM 인입 시 `EventID in (24577, 24588, 24620)`를 룰로 구성하고, PIN 실패( 이벤트 ID 851 )는 헬프데스크 알림으로 연계합니다.
- TPM 상태는 `Get-Tpm | Export-Clixml C:\\Logs\\TPM-state.xml`로 백업해 장애 원인 조사에 활용합니다.

### 로그 샘플
```text
PS C:\> manage-bde -status C:

BitLocker Drive Encryption: Configuration Tool version 10.0.22621

Volume "C:" [OSDisk]
    Size:                 237.87 GB
    Conversion Status:    Fully Encrypted
    Percentage Encrypted: 100.0%
    Encryption Method:    XTS-AES 256
    Protection Status:    Protection On
    Lock Status:          Unlocked
    Identification Field: -
    Key Protectors:
        TPM And PIN
        Numerical Password

PS C:\> manage-bde -protectors -get C:

BitLocker Drive Encryption: Configuration Tool version 10.0.22621

Volume "C:" [OSDisk]
All Key Protectors

    TPM And PIN:
      ID: {c43a8e51-9c07-4f1b-a7d1-1cab4d361ce8}
      PCR Validation Profile:
        0,2,4,8,11

    Numerical Password:
      ID: {c1a0f6d7-9076-4dd4-8cbb-a0d3474a21b5}
      Password:
        123456-789012-345678-901234-567890-123456-789012-345678

PS C:\> Get-WinEvent -LogName "Microsoft-Windows-BitLocker-API/Operational" -MaxEvents 3 | \
    Select-Object TimeCreated, Id, LevelDisplayName, Message

TimeCreated          Id LevelDisplayName Message
-----------          -- ---------------- -------
2025-09-26 09:12:04 24577 Information    BitLocker Drive Encryption successfully unlocked volume C: using TpmPinProtector.
2025-09-26 09:11:59 24588 Information    BitLocker Drive Encryption recovery password was backed up for volume C:.
2025-09-25 18:45:12 24620 Warning        BitLocker Drive Encryption detected a ClearKey protector on volume D:.

PS C:\> Get-Tpm | Select-Object TpmPresent,TpmReady,TpmVersion

TpmPresent TpmReady TpmVersion
---------- -------- ----------
      True     True 2.0

PS C:\> Get-BitLockerVolume | Format-List MountPoint,VolumeType,ProtectionStatus,EncryptionPercentage

MountPoint           : C:
VolumeType           : OperatingSystem
ProtectionStatus     : On
EncryptionPercentage : 100
```

- 텍스트/EVTX 로그는 `\\fileserver\security\bitlocker\YYYY\MM` 하위에 저장하고, 매주 무결성 해시(SHA-256)를 생성해 감사 기록에 첨부합니다.

## BitLocker To Go USB 예제
```powershell
# 이동식 드라이브(D:) 암호화 및 복구 키 파일 저장
Enable-BitLocker -MountPoint "D:" -EncryptionMethod XtsAes256 -PasswordProtector
Backup-BitLockerKeyProtector -MountPoint "D:" -File "\\fileserver\recovery\D_drive.bek"

# 정책 준수 확인
Get-BitLockerVolume -MountPoint "D:" | Format-Table -AutoSize
```

## 운영 체크리스트 요약
- [ ] TPM 상태 및 PCR 구성 검증 (`tpm.msc`, `Get-Tpm`)
- [ ] PIN 정책 최소 길이(예: 6 digits) 설정 및 사용자 안내
- [ ] 복구 키 백업 위치 점검(AD/Azure AD/탈기기 저장 금지)
- [ ] `drawio/bitlocker_flow.drawio` 다이어그램을 사용자/헬프데스크 교육 자료에 포함
- [ ] `Event Viewer` 자동 경고(24588, 24620) 모니터링 룰 구성

# 실무/시험 포인트
- 엔터프라이즈 환경에서는 GPO(`Computer Configuration > Administrative Templates > Windows Components > BitLocker Drive Encryption`)로 강제 정책을 배포합니다.
- 사고 대응을 위해 복구 키 회수 절차와 감사 로그(Event ID 24579 등)를 정기적으로 점검합니다.
- BitLocker와 VeraCrypt 비교 시 OS 통합, 하드웨어 가속, 중앙 관리 기능에서 차이가 있음.

# TODO / 후속 연구
- [ ] BitLocker 네트워크 언락(Network Unlock) 시나리오 정리.
- [ ] TPM 미탑재 환경에서 USB 스타트업 키를 사용하는 절차 캡처.
- [ ] BitLocker To Go 정책 템플릿 수집 후 링크화.
- [x] `drawio/bitlocker_flow.drawio`에 TPM+PIN 인증 흐름 다이어그램 작성 (2025-09-25)
- [ ] Azure AD/Intune 보고서 스크린샷 추가

# 참고 자료
- [Microsoft Learn – BitLocker 개요](https://learn.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview)
- [BitLocker 복구 키 관리 모범 사례](https://learn.microsoft.com/windows/security/operating-system-security/data-protection/bitlocker/bitlocker-keys)
