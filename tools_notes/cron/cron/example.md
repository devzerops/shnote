- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 기본 crobtab 
```bash
## /etc/crontab: system-wide crontab
## Unlike any other crontab you don't have to run the `crontab'
## command to install the new version when you edit this file
## and files in /etc/cron.d. These files also have username fields,
## that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

## Example of job definition:
## .---------------- minute (0 - 59)
## |  .------------- hour (0 - 23)
## |  |  .---------- day of month (1 - 31)
## |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
## |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
## |  |  |  |  |
## *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
##
```

23:45 (11:45 PM) 매 토요일마다 export_dump.sh 실행
```bash
45 23 * * 6 /home/oracle/scripts/export_dump.sh
```

5분마다 명령어 hello world 실행
```bash
*/5 1,2,3 * * * echo hello world
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
