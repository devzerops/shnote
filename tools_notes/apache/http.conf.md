- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## http.conf

| 보안 설정            	| 내용                                                                              	| 예시                                  	|
|----------------------	|-----------------------------------------------------------------------------------	|---------------------------------------	|
| ServerRoot           	| - 아파치의 기본 Root 경로                                                         	|                                       	|
| Timeout              	| - 클라이언트의 요청에 서버가 대기하는 시간을 설정                                 	| Timeout 300                           	|
| MaxKeepAliveRequests 	| - 접속을 허용할 수 있는 최대 횟수                                                 	| Maxkeepaliverequest 100               	|
| KeepAliveTimeout     	| - 클라이언트 최초 요청을 받은 뒤에 다음 요청이 전송될 때까지 대기하는 시간을 설정 	| KeepAliveTimeout 100                  	|
| KeepAlive            	| - 접속 연결에 대한 재용청을 허용할 것인지 설정                                    	| Keepalive On                          	|
| Directoryindex       	| - 웹 디렉토리 접근 시 인식되는 인덱스 파일의 순서 지정                            	| DirectoryIndex index.htm index.php    	|
| Listem               	| - 사용할 포트 지정                                                                	| Listen 1120                           	|
| Servername           	| - 서버의 도메인 지정                                                              	| Servername zidarn87.tistory.com:1120  	|
| DocumentRoot         	| - 웹문서가 위치하는 디렉토리 지정                                                 	| DocumentRoot "/var/www/html/webadmin" 	|
| ServerAdmin          	| - 서버 관리자 이메일 지정                                                         	|                                       	|
| CustomLog            	| - 엑세스 로그 파일 위치 지정                                                      	|                                       	|
| ErrorLog             	| - 에러 로그 파일의 위치 지정                                                      	|                                       	|
| %>S                  	| 마지막 응답의 상태                                                                	|                                       	|
| %t                   	| 시간                                                                              	|                                       	|
| %T                   	| 처리하는데 걸린 시간                                                              	|                                       	|
| %u                   	| 인증 사용자명                                                                     	|                                       	|
| %U                   	| 요청의 URL 경로                                                                   	|                                       	|
| %v                   	| 요청에 대한 가상 호스트 이름                                                      	|                                       	|
| %V                   	| UseCanonicalName 따른 서버명.                                                     	|                                       	|
| %X                   	| 연결 상태                                                                         	|                                       	|
| %{쿠키명}C           	| 요청에 포함된 쿠키 값                                                             	|                                       	|
| % {환경변수명}e      	| 환경 변수명의 값                                                                  	|                                       	|
| %{헤더명}i           	| 요청에 포함된 헤더명의 값                                                         	|                                       	|
| %{헤더명}o           	| 응답에 포함된 헤더명의 값                                                         	|                                       	|
| %{메모}n             	| 모듈에서 반환한 메모의 값                                                         	|                                       	|
| %{포맷}t             	| 포맷된 시간                                                                       	|                                       	|

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
