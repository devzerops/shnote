- 마지막 업데이트: 2025-09-26
- 상태: 초안

# 개요
VeraCrypt는 오픈소스 디스크 암호화 도구로, 파일 컨테이너·파티션·전체 디스크를 암호화할 수 있습니다. GUI와 CLI를 모두 제공하며, 휴대용 볼륨 생성과 교차 OS 환경(Windows/Linux/macOS)에서의 호환성이 특징입니다.

## CLI 기반 볼륨 생성 절차
1. **컨테이너 생성**
   ```bash
   veracrypt --text --create /secure/notes.vc \
     --size=2G --hash=sha512 --encryption=aes --filesystem=ext4 \
     --password="${VC_PASS}" --non-interactive
   ```
   - `--size`: 필요 용량을 지정합니다. 단위는 자동으로 인식(예: `2G`).
   - `--filesystem`: Linux에서는 `ext4`, Windows 공유용은 `ntfs` 또는 `exfat` 권장.
2. **마운트**
   ```bash
   sudo veracrypt --text /secure/notes.vc /mnt/veracrypt1 \
     --password="${VC_PASS}" --pim=0 --non-interactive
   ```
   - 시스템-wide 접근을 위해 `sudo` 사용. 사용자 전용이면 루프백 장치를 활용할 수 있습니다.
3. **마운트 확인 & 권한 조정**
   ```bash
   veracrypt --text --list
   sudo chown $USER:$USER /mnt/veracrypt1
   ```
4. **언마운트**
   ```bash
   sudo veracrypt --text --dismount /mnt/veracrypt1
   ```

## 로그 및 검증 포인트
- `veracrypt --text --list` 출력과 `/proc/mounts` 스냅샷을 캡처해 감사 로그로 보관합니다.
- 생성 직후 `lsblk -f`로 파일시스템과 암호화 계층을 확인해두면 장애 조사 시 도움이 됩니다.
- 실패 로그는 `~/.VeraCrypt`와 `journalctl --unit=veracrypt-*`에서 확인합니다.

### 로그 샘플
```text
# veracrypt --text --list
1: /secure/notes.vc /dev/mapper/veracrypt1 /mnt/veracrypt1 ext4 RW
   Size: 2.0 GiB
   Encryption Algorithm: AES
   PKCS5 PRF: HMAC-SHA-512

# lsblk -f /dev/mapper/veracrypt1
NAME        FSTYPE LABEL     UUID                                 MOUNTPOINT
veracrypt1  ext4   VC_NOTES  1c2f3a4b-5678-4a9a-9d3c-13579bd24680 /mnt/veracrypt1

# systemctl status veracrypt-device@secure-notes.vc.service
● veracrypt-device@secure-notes.vc.service - VeraCrypt volume /secure/notes.vc
     Loaded: loaded (/etc/systemd/system/veracrypt-device@.service; enabled)
     Active: active (mounted) since Fri 2025-09-26 10:04:13 KST; 2h 31min ago
       Docs: https://www.veracrypt.fr/en/Documentation.html
   Main PID: 2134 (veracrypt)

# journalctl --unit=veracrypt-device@secure-notes.vc --since "2025-09-26 10:00"
Sep 26 10:04:12 ops-host systemd[1]: Starting VeraCrypt volume /secure/notes.vc...
Sep 26 10:04:13 ops-host veracrypt[2134]: Mounted /secure/notes.vc at /mnt/veracrypt1 (AES+SHA512, hotkey disabled)
Sep 26 18:32:45 ops-host veracrypt[9821]: Dismounted /mnt/veracrypt1 (user=opsadmin)

# dmesg | tail -n 4
[56324.437821] veracrypt1: detected filesystem type ext4
[56324.439110] EXT4-fs (veracrypt1): mounted filesystem with ordered data mode
```

- 로그 파일은 날짜 기반으로 `logs/veracrypt/YYYY/MM/DD-veracrypt.log`에 보관하고, 마운트/언마운트 시간·실행 계정·호스트명을 CSV로 정리하면 다중 서버 비교가 수월하다.

## 자동 마운트 스크립트 예시
```bash
#!/usr/bin/env bash
set -euo pipefail
VOL_PATH="/secure/notes.vc"
MOUNT_POINT="/mnt/veracrypt1"
PASS_FILE="/run/secrets/veracrypt_pass"

if ! [ -f "$PASS_FILE" ]; then
  echo "[!] Password file not found" >&2
  exit 1
fi

sudo veracrypt --text "$VOL_PATH" "$MOUNT_POINT" \
  --password="$(<"$PASS_FILE")" --non-interactive --pim=0
```
> *TIP*: 비대화형 마운트 시 비밀 값은 파일·환경변수 대신 `systemd-ask-password` 또는 하드웨어 토큰 연계를 고려합니다.

## 보안 운영 체크리스트
- [ ] 마스터 키 백업: `VeraCrypt Volume` 생성 시 복구 키/헤더 백업 파일을 별도 저장소에 보관.
- [ ] Passphrase 순환 정책: 분기별 변경 주기 수립, 공유 저장 금지.
- [ ] 마운트 자동화: systemd 서비스(디바이스 단위) 작성 및 `Before=` 의존성으로 응용프로그램 실행 순서 컨트롤.
- [ ] 감사 로그: 마운트/언마운트 시간, 실행 계정을 `auditd` 규칙으로 기록.

## TODO / 후속 연구
- [ ] GUI 마법사 단계별 스크린샷과 CLI 비교표 추가.
- [ ] Windows CLI(`veracrypt.exe /create`) 파라미터 매핑 문서화.
- [ ] `veracrypt --protect-hidden` 옵션 테스트 후 안전한 사용 가이드 작성.

## 참고 자료
- VeraCrypt 공식 문서: <https://www.veracrypt.fr/en/Documentation.html>
- VeraCrypt CLI Guide: `/usr/share/doc/veracrypt/HTML/EN/Command%20Line%20Usage.html`
- PortMaster Blog – *Automating VeraCrypt mounts on Linux*.
