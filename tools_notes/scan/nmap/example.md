- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
단일 IP 스캔

nmap 192.168.100.128



특정 IP 스캔

nmap 192.168.100.128 192.168.100.129



호스트 스캔

nmap www.koromoon.com



IP 범위 스캔

nmap 192.168.100.128-254



서브넷 스캔

nmap 192.168.100.0/24



텍스트 파일에서 대상 스캔

nmap -iL koromoon_ip_list.txt



2. 포트 선택



단일 포트 스캔

nmap -p 21 192.168.100.128



잘 알려진 포트 스캔

nmap -p 1-1023 192.168.100.128



100 개의 잘 알려진 빠른 포트 스캔

nmap -F 192.168.100.128



서비스 이름으로 포트 스캔

nmap -p http,https 192.168.100.128



모든 포트 스캔

nmap -p- 192.168.100.128



지정된 UDP 및 TCP 포트 스캔

nmap -p U:53,T:21-25,80 192.168.100.128



3. 스캔 유형



TCP 연결을 사용하여 스캔

nmap -sT 192.168.100.128



TCP SYN 스캔을 사용하여 스캔

nmap -sS 192.168.100.128



UDP 포트 스캔

nmap -sU -p 123,161,162 192.168.100.128



선택한 포트 검색 (검색 무시)

nmap -Pn -F 192.168.100.128



4. 서비스 및 OS 감지



OS 및 서비스 감지

nmap -A 192.168.100.128



표준 서비스 감지

nmap -sV 192.168.100.128



공격적인 서비스 감지 (범위는 0 ~ 9, 범위가 높을수록 정확성이 높아짐)

nmap -sV –version-intensity 5 192.168.100.128



5. 결과물 형식



기본 결과물을 파일에 저장

nmap -oN result.txt 192.168.100.128



결과물을 XML 로 저장

nmap -oX result.xml 192.168.100.128



형식화된 결과물로 저장 (Grepable)

nmap -oG result.txt 192.168.100.128



모든 형식으로 결과물 저장

nmap -oA result 192.168.100.128



6. 스크립팅 엔진



기본 안전 스크립트를 사용하여 스캔

nmap -sV -sC 192.168.100.128



스크립트에 대한 도움말보기

nmap --script-help http-wordpress-enum



특정 스크립트(SQL Injection)를 사용하여 스캔

nmap -p 80 -script=http-sql-injection 192.168.100.128



HTTP 관련 스크립트로만 스캔(엄청 오래 걸림)

nmap -p 80 --script=http* 192.168.100.131



UDP DDOS 반사공격 가능한 좀비 검색

nmap -sU -A -PN -n -pU:19,53,123,161 -script=ntp-monlist,dns-recursion,snmp-sysdescr 192.168.100.0/24



스크립트 데이터베이스 업데이트

nmap –script-updatedb



7. 방화벽/IDS 회피 및 스푸핑



작은 조각화된 IP 패킷 사용하는 스캔(ping 스캔 포함)

nmap -f 192.168.100.128



자신만의 오프셋 크기 설정해서 스캔

nmap --mtu 32 192.168.100.128



스푸핑된 IP 에서 스캔 보내기 (-D 옵션값들은 조작된 IP 임)

nmap -D 192.168.100.1,192.168.100.2,192.168.100.3,192.168.100.4 192.168.100.128



주어진 소스 포트 번호 사용

nmap -g 53 192.168.100.128



HTTP/SOCKS4 프록시를 통한 릴레이 연결

nmap --proxies http://www.koromoon.com:8080, http://www.koromoon2.com:8080 192.168.100.128



전송된 패킷에 임의의 데이터를 추가하는 스캔

nmap --data-length 200 192.168.100.128



고도화된 IDS 회피 스캔


nmap -f -t 0 -n -Pn –data-length 200 -D 192.168.100.1,192.168.100.2,192.168.100.3,192.168.100.4 192.168.100.128

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
