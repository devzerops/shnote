- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## Cron
* 리눅스의 자동화를 도와주는 툴

## crontab 명령어 설정
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

## 필드
|     Field    	| Required 	|  Allowed values 	| Allowed special characters 	|                              Remarks                             	|
|:------------:	|:--------:	|:---------------:	|:--------------------------:	|:----------------------------------------------------------------:	|
| Minutes      	|    Yes   	| 0–59            	| * , -                      	|                                                                  	|
| Hours        	|    Yes   	| 0–23            	| * , -                      	|                                                                  	|
| Day of month 	|    Yes   	| 1–31            	| * , - ? L W                	| ? L W only in some implementations                               	|
| Month        	|    Yes   	| 1–12 or JAN–DEC 	| * , -                      	|                                                                  	|
| Day of week  	|    Yes   	| 0–6 or SUN–SAT  	| * , - ? L #                	| ? L # only in some implementations                               	|
| Year         	|    No    	| 1970–2099       	| * , -                      	| This field is not supported in standard/default implementations. 	|

## 표현식
*  \* : 모든 값을 뜻합니다.
*  ? : 특정한 값이 없음을 뜻합니다. 
*  - : 범위를 뜻합니다. (예) 월요일에서 수요일까지는 MON-WED로 표현
*  , : 특별한 값일 때만 동작 (예) 월,수,금 MON,WED,FRI 
*  / : 시작시간 / 단위  (예) 0분부터 매 5분 0/5
*  L : 일에서 사용하면 마지막 일, 요일에서는 마지막 요일(토요일)
*  W : 가장 가까운 평일 (예) 15W는 15일에서 가장 가까운 평일 (월 ~ 금)을 찾음
*  # : 몇째주의 무슨 요일을 표현 (예) 3#2 : 2번째주 수요일


## 매크로

|          Entry         	|                         Description                        	| Equivalent to 	|
|:----------------------:	|:----------------------------------------------------------:	|:-------------:	|
| @yearly (or @annually) 	| Run once a year at midnight of 1 January                   	| 0 0 1 1 *     	|
| @monthly               	| Run once a month at midnight of the first day of the month 	| 0 0 1 * *     	|
| @weekly                	| Run once a week at midnight on Sunday morning              	| 0 0 * * 0     	|
| @daily (or @midnight)  	| Run once a day at midnight                                 	| 0 0 * * *     	|
| @hourly                	| Run once an hour at the beginning of the hour              	| 0 * * * *     	|
| @reboot                	| Run at startup                                             	|       —       	|
|       연도 (Year)      	|                    empty or 1970 ~ 2099                    	|    , - * /    	|

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
