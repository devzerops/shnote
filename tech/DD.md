- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
Rufus 등 이미지 굽기 도구에서 `DD`(Direct Disk) 모드는 원본 ISO 이미지를 섹터 단위로 대상 디스크에 그대로 복제합니다. 표준 파일 시스템을 구성하지 않고 RAW 상태로 덤프하기 때문에 호환성과 복구 관점에서 ISO 모드와 다른 특성을 가집니다.

# 핵심 개념
- **섹터 단위 복제**: 부트 레코드, 파티션 테이블까지 1:1로 복사하므로 설치 미디어와 완전히 동일한 구조를 재현합니다.
- **호환성**: UEFI/Legacy BIOS와 상관없이 대부분의 펌웨어에서 인식되지만, Windows에서는 마운트 시 파일이 보이지 않을 수 있습니다.
- **쓰기 단위**: 대상 드라이브 전체가 덮어쓰기 되므로 기존 데이터는 복구가 어렵습니다.
- **무결성 검증**: 원본 ISO와 대상 장치에 대해 `sha256sum`을 수행해 결과가 일치하는지 확인합니다. 이미지 신뢰성 확보가 목적이라면 해시 비교 로그를 보관합니다.

# 절차 예시 (Rufus DD 모드)
1. **ISO 선택**: Rufus에서 설치 ISO를 선택하고, `Image option`을 `DD`로 지정합니다. (Rufus FAQ: "Why is ISO vs DD mode important?")
2. **장치 백업**: Linux/WSL 환경이라면 `lsblk`와 `dd if=/dev/sdX of=backup.img bs=4M status=progress`로 백업 이미지를 확보합니다.
3. **DD 굽기 실행**: Rufus에서 진행하거나 CLI에서는 `sudo dd if=./install.iso of=/dev/sdX bs=4M status=progress oflag=sync`를 사용합니다.
4. **무결성 검증**:
   ```bash
   sudo dd if=/dev/sdX bs=4M count=$(($(stat -c%s install.iso)/4194304+1)) | sha256sum
   sha256sum install.iso
   ```
   두 해시가 일치하는지 확인하고, 로그를 `logs/dd/`에 보관합니다.
5. **안전 제거**: `sync` 후 장치를 분리합니다. Windows에서는 "하드웨어 안전하게 제거" 기능을 사용합니다.

# 복구/초기화 절차
1. `diskpart` → `list disk` → `select disk <n>` → `clean` → `convert gpt` → `create partition primary` → `format fs=fat32 quick`.
2. Linux에서는 `sudo wipefs -a /dev/sdX` 후 `parted` 또는 `fdisk`로 새 파티션 테이블을 생성합니다.
3. 백업 이미지를 되살릴 경우 `dd if=backup.img of=/dev/sdX bs=4M status=progress oflag=sync`.

# 실무/시험 포인트
- 설치 실패 시 ISO 모드→DD 모드 전환이 유효한 트러블슈팅 단계입니다.
- DD 모드로 생성한 USB는 복구 시 `diskpart clean` 등의 초기화 절차가 필요합니다.
- 포렌식 수집에는 `dd if=/dev/sdX of=image.dd bs=4M`와 같이 동일 개념을 활용하며, 해시 검증이 필수입니다.
- Smartctl로 대상 디스크 상태를 확인하고, 굽기 전 후로 `smartctl -H /dev/sdX` 결과를 기록하면 이슈 조사에 도움이 됩니다.
- dd 실행 시 `status=progress`와 `oflag=sync`를 지정해 진행 상황과 버퍼 쓰기를 안정적으로 관리합니다.

# TODO / 후속 연구
- Rufus CLI 스크린샷과 `dd` 명령 예제를 추가.
- DD/ISO 모드 비교 표 작성 및 `tech/ISO.md`와 상호 링크.
- DD 모드로 생성한 USB 복구 절차를 단계별로 문서화.
- `drawio/iso_vs_dd_flow.drawio`에 설치 흐름 비교 다이어그램 작성 후 문서에 포함.

# 참고 자료
- [Rufus 공식 FAQ – ISO vs DD 모드](https://rufus.ie/en/#faq)
- [GNU Coreutils `dd` 매뉴얼](https://www.gnu.org/software/coreutils/manual/html_node/dd-invocation.html)
- [NIST SP 800-101 Rev.1 – Digital Forensics Guidelines](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-101r1.pdf)
