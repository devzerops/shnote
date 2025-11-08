- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
| 포트		| TCP/UDP	|                                             설명												|
|:----:	|:-------:	|:--------------------------------------------------------------------------------------------:	|
|   0	|   UDP		|                                     예약됨; 사용하지 않음										|
|   1	|   TCP		|                              TCPMUX (TCP 포트 서비스 멀티플렉서)								|
|   7	| TCP/UDP	|                                         ECHO 프로토콜											|
|   9	| TCP/UDP	|                                       DISCARD 프로토콜										|
|  13	| TCP/UDP	|                                       DAYTIME 프로토콜										|
|  17	|   TCP		|                               QOTD (Quote of the Day) 프로토콜								|
|  19	| TCP/UDP	|                    CHARGEN (Character Generator) 프로토콜 - 원격 오류 수정					|
|  20	|   TCP		|                            FTP (파일 전송 프로토콜) - 데이터 포트								|
|  21	|   TCP		|                                        FTP - 제어 포트										|
|  22	|   TCP		|                SSH (Secure Shell) - ssh scp, sftp같은 프로토콜 및 포트 포워딩					|
|  23	|   TCP		|                          텔넷 프로토콜 - 암호화되지 않은 텍스트 통신							|
|  24	|   TCP		|                                        개인메일 시스템										|
|  25	|   TCP		|                   SMTP (Simple Mail Transfer Protocol) - 이메일 전송에 사용					|
|  37	| TCP/UDP	|                                         TIME 프로토콜											|
|  49	|   UDP		|                                        TACACS 프로토콜										|
|  53	| TCP/UDP	|                                   DNS (Domain Name System)									|
|  67	|   UDP		|                        BOOTP (부트스트랩 프로토콜) 서버. DHCP로도 사용						|
|  68	|   UDP		|                     BOOTP (부트스트랩 프로토콜) 클라이언트. DHCP로도 사용						|
|  69	|   UDP		|                                             TFTP												|
|  70	|   TCP		|                                         고퍼 프로토콜											|
|  79	|   TCP		|                                        Finger 프로토콜										|
|  80	| TCP/UDP	|                      HTTP (HyperText Transfer Protocol) - 웹 페이지 전송						|
|  88	|   TCP		|                                   커베로스 - 인증 에이전트									|
|  109	|   TCP		|               POP2 (Post Office Protocol version 2) - 전자우편 가져오기에 사용				|
|  110	|   TCP		|               POP3 (Post Office Protocol version 3) - 전자우편 가져오기에 사용				|
|  111	| TCP/UDP	|                                  RPC (Remote Procedure Call)									|
|  113	|   TCP		|             ident - 예전 서버 인증 시스템, 현재는 IRC 서버에서 사용자 인증에 사용				|
|  119	|   TCP		|           NNTP (Network News Transfer Protocol) - 뉴스 그룹 메시지 가져오기에 사용				|
|  123	|   UDP		|                           NTP (Network Time Protocol) - 시간 동기화							|
|  139	|   TCP		|                                          넷바이오스											|
|  143	|   TCP		|                IMAP4 (인터넷 메시지 접근 프로토콜 4) - 이메일 가져오기에 사용					|
|  161	|   UDP		|                    SNMP (Simple Network Management Protocol) - Agent 포트						|
|  162	|   UDP		|                                      SNMP - Manager 포트										|
|  179	|   TCP		|                                 BGP (Border Gateway Protocol)									|
|  194	|   TCP		|                                   IRC (Internet Relay Chat)									|
|  389	|   TCP		|                         LDAP (Lightweight Directory Access Protocol)							|
|  443	|   TCP		|                              HTTPS - SSL 위의 HTTP (암호화 전송)								|
|  445	|   TCP		|           Microsoft-DS (액티브 디렉터리, 윈도 공유, Sasser-worm, Agobot, Zobotworm)			|
|  445	|   UDP		|                                  Microsoft-DS SMB 파일 공유									|
|  465	|   TCP		|                             SSL 위의 SMTP - Cisco 프로토콜과 충돌								|
|  514	|   UDP		|                              syslog 프로토콜 - 시스템 로그 작성								|
|  515	|   TCP		|                            LPD 프로토콜 - 라인 프린터 데몬 서비스								|
|  540	|   TCP		|                               UUCP (Unix-to-Unix Copy Protocol)								|
|  542	| TCP/UDP	| 상용 (Commerce Applications) <br>(RFC maintained by: Randy Epstein [repstein at host.net])	|
|  587	|   TCP		|                          email message submission (SMTP) (RFC 2476)							|
|  591	|   TCP		|                   파일메이커 6.0 Web Sharing (HTTP Alternate, see port 80)					|
|  636	|   TCP		|                                 SSL 위의 LDAP (암호화된 전송)									|
|  666	|   TCP		|                             id 소프트웨어의 둠 멀티플레이어 게임								|
|  873	|   TCP		|                                  rsync 파일 동기화 프로토콜									|
|  981	|   TCP		|     SofaWare Technologies Checkpoint Firewall-1 소프트웨어 내장 방화벽의 원격 HTTPS 관리			|
|  990	|   TCP		|                                  SSL 위의 FTP (암호화 전송)									|
|  992	|   TCP		|                                 SSL 위의 Telnet (암호화 전송)									|
|  993	|   TCP		|                                 SSL 위의 IMAP4 (암호화 전송)									|
|  995	|   TCP		|                                  SSL 위의 POP3 (암호화 전송)									|

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
