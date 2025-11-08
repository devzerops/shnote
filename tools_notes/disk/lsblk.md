- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## disk 검사 도구
``` bash
sudo lsblk -o name,mountpoint,label,size,uuid
```


NAME   MOUNTPOINT        LABEL                             SIZE UUID
loop0  /snap/core18/2620                                  55.6M
loop1  /snap/core18/2632                                  55.6M
loop2  /snap/docker/2285                                 141.4M
loop3  /snap/snapd/17883                                  49.6M
sda                                                         25G
├─sda1 /boot/efi                                             1G D13E-DCA9
├─sda2 [SWAP]                                                1G db453f0e-446a-47e9-bd02-fcc2a57b8af9
└─sda3 /                                                  22.9G af25fdbb-bed3-4710-baf4-b0c48c777b97
sr0                      Ubuntu-Server 22.04.1 LTS amd64   1.4G 2022-08-09-16-48-33-00

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
