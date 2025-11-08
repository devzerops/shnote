- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 주소 결정 프로토콜(Address Resolution Protocol, ARP)  
네트워크 상에서 IP 주소를 물리적 네트워크 주소로 대응(bind)시키기 위해 사용되는 프로토콜이다. 여기서 물리적 네트워크 주소는 이더넷 또는 토큰링의 48 비트 네트워크 카드 주소를 뜻한다. ARP는 1982년 인터넷 표준 STD 37인 "RFP 826"[1]에 의해 정의되었다.

이를테면, IP 호스트 A가 IP 호스트 B에게 IP 패킷을 전송하려고 할 때 IP 호스트 B의 물리적 네트워크 주소를 모른다면, ARP 프로토콜을 사용하여 목적지 IP 주소 B와 브로드캐스팅 물리적 네트워크 주소 FFFFFFFFFFFF를 가지는 ARP 패킷을 네트워크 상에 전송한다. IP 호스트 B는 자신의 IP 주소가 목적지에 있는 ARP 패킷을 수신하면 자신의 물리적 네트워크 주소를 A에게 응답한다.

이와 같은 방식으로 수집된 IP 주소와 이에 해당하는 물리적 네트워크 주소 정보는 각 IP 호스트의 ARP 캐시라 불리는 메모리에 테이블 형태로 저장된 다음, 패킷을 전송할 때에 다시 사용된다. ARP와는 반대로, IP 호스트가 자신의 물리 네트워크 주소는 알지만 IP 주소를 모르는 경우, 서버로부터 IP주소를 요청하기 위해 RARP를 사용한다.
## 헤더
* Hardware Type (HTYPE)  : 네트워크 유형을 정의하며, Ethernet 환경의 경우 0x0001으로 세팅
* Protocol Type (PTYPE)  : 프로토콜을 정의하며, IP 프로토콜 버전4(IPv4)의 경우 0x0800 세팅
* Hardware Length (HLEN)  : MAC주소의 길이를 정의하며, Ethernet 환경의 경우 6 byte 세팅
* Protocol Length (PLEN)  : 프로토콜의 길이를 정의하며, IPv4의 경우 4 byte 세팅
* Operation (OPER)  : 패킷의 유형이며, ARP 요청(ARP Request)의 경우 1, ARP 응답 (ARP Reply)의 경우 2 세팅
* Sender Hardware Address (SHA)  : 발신자의 MAC 주소 세팅
* Sender Protocol Address (SPA)  : 발신자 IP 주소 세팅
* Target Hardware Address (THA)  : 목적지 MAC 주소, 그러나 ARP Request의 경우 알 수 없음
* Target Protocol Address (TPA)  : 목적지 IP 주소 세팅

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
