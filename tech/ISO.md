- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
ISO 이미지는 CD/DVD 등 광학 매체 구조를 그대로 담은 단일 파일 포맷입니다. 운영체제 설치 미디어 제작, 배포용 콘텐츠 패키징, 보안 시험 환경 구축 등에 널리 활용됩니다.

# 핵심 개념
- **파일 시스템**: ISO 9660/Joliet/UDF 등 규격으로 구성되며, 하위 디렉터리와 파일 메타데이터를 포함합니다.
- **부트 구조**: El Torito 규격을 통해 BIOS/UEFI 부팅을 지원하며, 하이브리드 ISO는 USB 플래시에 쓰기 위해 추가 파티션을 포함합니다.
- **배포 편의성**: 단일 파일로 패키징되므로 서명·무결성 검증(예: SHA256)이 용이합니다.
- **Rock Ridge/Joliet 확장**: POSIX 권한, 긴 파일명을 지원하기 위해 ISO 9660 위에 확장 필드를 사용합니다.

# 제작/검증 절차
1. **이미지 생성** (`mkisofs`/`genisoimage`):
   ```bash
   genisoimage -o distro.iso -V "MYDISTRO" -J -R -b boot/isolinux.bin \
     -c boot/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table ./iso_root
   ```
2. **UEFI 부트 가능 이미지**: `xorriso -as mkisofs ... -eltorito-alt-boot -e EFI/boot/bootx64.efi -no-emul-boot` 옵션으로 EFI 파티션을 포함.
3. **무결성 검증**: SHA256/PGP 서명을 제공하는 벤더 사이트에서 체크섬과 서명 파일을 내려받아 `sha256sum -c`, `gpg --verify`로 검증.
4. **마운트 테스트**: Linux `sudo mount -o loop distro.iso /mnt/iso`; macOS `hdiutil attach distro.iso`; Windows `PowerShell Mount-DiskImage`.
5. **USB에 쓰기**: 일반적인 설치용은 Rufus ISO 모드 권장. 베어메탈 호환성 이슈가 있을 경우 `tech/DD.md`의 절차를 따른다.

# 분석/보안 주의사항
- Emotet 등 악성 ISO 사례는 Office 문서를 ISO에 패키징해 실행하도록 유도하므로, ISO 파일 실행 전 반드시 Sandboxing 및 정적 분석을 수행한다 (MSRC, CISA 권고).
- `isoinfo -d -i file.iso`로 볼륨 헤더, `isoinfo -f -i file.iso`로 파일 목록을 확인해 예상치 못한 실행 파일이나 자동 실행 스크립트를 검토한다.
- EU GDPR 등 규제 환경에서는 ISO 배포 시 개인정보 포함 여부를 검토하고, 필요하면 암호화된 컨테이너로 배포한다.

# 실무/시험 포인트
- ISO 이미지를 USB로 굽는 경우 Rufus/Etcher 등에서 ISO 모드와 DD 모드의 차이를 이해해야 합니다.
- 악성코드 유포에도 활용되므로 첨부된 ISO는 마운트 전 SHA256 검증과 샌드박스 분석이 필요합니다.
- Windows 10 이후는 ISO 파일을 기본적으로 마운트할 수 있지만, 구형 OS는 Daemon Tools 등 별도 툴을 요구합니다.
- VMware/VirtualBox 등 가상화 환경에 배포할 때는 ISO를 직접 마운트하거나 `virt-install --location` 옵션을 사용하면 PXE 없이 설치 자동화가 가능합니다.

# TODO / 후속 연구
- ISO/UDF 구조 다이어그램을 `drawio/iso_layers.drawio`로 작성.
- `isoinfo`, `hdiutil` 등 도구 명령 예시 추가.
- 악성 ISO 사례(Emotet 등) 조사 후 참고 링크화.
- DD 모드와 비교 내용을 `tech/DD.md`와 교차 링크.

# 참고 자료
- [ISO 9660 개요 – ECMA-119](https://www.ecma-international.org/publications-and-standards/standards/ecma-119/)
- [Microsoft Learn – Windows 10에서 ISO 마운트](https://support.microsoft.com/windows/mount-an-iso-image)
- [El Torito 부트 규격 요약](https://wiki.osdev.org/El-Torito)
- [xorriso Manual – GNU libisoburn](https://www.gnu.org/software/xorriso/)
- [CISA Alert AA22-121A – Malicious ISO Distribution](https://www.cisa.gov/news-events)
