- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 부팅할때마다 실행

``` bash
cron -e -i 
```


crontab -e -i 안에서
(아래 명령어는 보안상 취약해서 사용을 비추천함 예시일 뿐)

`etc/init.d`
`/etc/rc.local`

init.d -> crontab -> rc.local순 실행

``` bash
@reboot echo "1234" | sudo -S service ssh restart
```

``` bash
@reboot /root/backup.sh
```

## 만약 service를 부팅해도 유지하고싶다면은

``` bash
sudo systemctl enabled ssh 
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
