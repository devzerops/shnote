- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## DNS(Domain Name System)

DNS
정변환: DNS를 통해 IP를 알아냄
역변환: IP를 통해 DNS를 알아냄

## DNS 동작 방식
![DNS](https://www.lesstif.com/linux-core/files/125304855/129008929/1/1642003660000/image2022-1-13_1-7-38.png)

1. 사용자가 docs.google.com IP 를 재귀적 DNS 에 요청
2. 재귀적 DNS 는 Root DNS 에 .com 에 대한 TLD DNS 정보 요청
3. Root DNS 는 TLD DNS 정보 전송
4. 재귀적 DNS 는 TLD DNS 에 google.com 에 대한 권한있는 네임 서버 정보 요청
5. TLD DNS 는 google.com 를 관리하는 권한있는 네임 서버 정보 전송
6. 재귀적 DNS 는 권한있는 DNS에 docs.google.com 서버의 IP 정보 요청
7. 권한있는 DNS 는 재귀적 DNS 에 IP 정보 전달
8. 재귀적 DNS 는 사용자에게 docs.google.com 의 IP 정보 전달
9. 사용자는 docs.google.com 에 연결 요청
10. docs.google.com 에 연결됨

## 주요 레코드

| 레코드 유형          | 설명                                                                                                                                        |
|-------------------  |---------------------------------------------------------------------------------------------------------------------------------------------|
| A(address)          | 도메인 이름에 대한 IP 주소를 지정합니다.                                                                                                    |
| AAAA                | IPv6 주소를 지정합니다.                                                                                                                     |
| MX (Mail Exchanger) | 도메인 이름과 관련된 메일 서버의 정보를 지정합니다.                                                                                         |
| CNAME               | 도메인 이름을 다른 도메인 이름에 연결합니다.                                                                                                |
| NS                  | 도메인 이름과 관련된 네임 서버의 정보를 지정합니다.                                                                                         |
| TXT                 | 텍스트 정보를 저장하는 레코드입니다. 주로 SPF(Sender Policy Framework)나 DKIM(DomainKeys Identified Mail)의 인증 정보를 저장합니다.         |
| PTR                 | IP 주소에 대한 도메인 이름을 지정합니다.                                                                                                    |
| SRV                 | 도메인 이름과 관련된 서비스의 정보를 지정합니다.                                                                                            |
| HINFO               | 호스트 정보를 저장합니다. 일반적으로 호스트의 CPU 타입, 운영 체제 등을 저장합니다.                                                          |
| SOA                 | `Start of Authority`의 약자로, DNS 영역에 대한 정보를 저장합니다. 도메인 이름에 대한 primary 네임 서버, 메일 주소, 변경 내역 등을 저장합니다. |
| PTR                 | 역방향 DNS 레코드로, `IP 주소에 대한 도메인 이름을 지정합니다.` 일반적으로 IP 주소의 뒤에서부터 역순으로 도메인 이름을 지정합니다.            |
| NS                  | 도메인 이름과 관련된 네임 서버의 정보를 지정합니다. 일반적으로 도메인 이름에 대한 primary와 secondary 네임 서버의 정보를 저장합니다.        |
|ISDN                 | (Integrated Services Digital Network) ISDN 리소스 레코드는 호스트의 ISDN 주소를 알려줍니다. <br>ISDN 주소는 국가 코드, 국가 별 대상 코드, ISDN 가입자 번호 및 선택적으로 ISDN 하위 주소로 구성된 전화 번호입니다. <br>레코드의 기능은 A 레코드 기능의 변형 일뿐입니다.|
| AXFR | 존 버전에 상관없이 무조건 존 전송을 요청한다. |
| IXFR | 존 버전을 비교하여 상위 버전일 경우 존 전송을 요청한다. |

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
