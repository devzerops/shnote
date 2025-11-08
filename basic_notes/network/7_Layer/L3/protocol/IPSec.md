- 마지막 업데이트: 2025-09-25
- 상태: 검토중

# 개요
## IPSec 모드
![IPSec](../../img/IPSec.png)

## AH(Authentication Header)
* `인증 서비스`, 비연결형, 무결성, 재연(reply) 공격 방지 서비스를 제공
* AH 프로토콜은 발신지 호스트와 메시지에 대한 인증과 IP 패킷의 페이로드의 무결성을 제공하기 위해 사용된다.
* AH 프로토콜의 경우 `기밀성을 보장하지 못한다`.

## ESP(Encapsulation Security Payload)
* 비연결형 기밀성(암호화), 무결성, 인증 서비스를 제공한다.
* ESP 프로토콜은 IP 데이터그램에서 제공하는 선택적 인증과 무결성, 기밀성 그리고 `재전송 공격 방지` 기능을 한다.  터널 종단 간에 협상된 키와 암호화 알고리즘으로 데이터그램을 암호화한다.
* ESP 프로토콜의 경우 암호화 알고리즘으로 DES, 3DES, AES등을 사용할 수 있다.
* ESP 프로토콜은 인증을 사용하지 않을수도 있다.
* ESP는 전송 및 터널 모드를 지원한다.

| Protocol                             	| Confidentiality 	| Integrity 	| Authentication 	| Anti-replay 	| Header Size 	|
|--------------------------------------	|-----------------	|-----------	|----------------	|-------------	|-------------	|
| Authentication Header (AH)           	| No              	| Yes       	| Yes            	| Yes         	| Variable    	|
| Encapsulating Security Payload (ESP) 	| Yes             	| Yes       	| Yes            	| Yes         	| Variable    	|

## 트랜스포트 모드(Transport Mode)
![transport](../../img/transport.png)
## 특징
* 호스트와 호스트간 통신을 보호하기 위해 사용한다.
* IP 상위의 프로토콜 정보(주로 트랜스포트 프로토콜의 정보)를 인터넷을 통해 안전하게 전달한다.
* IP 헤더 다음에 IPsec 헤더 정보로 추가한다.
* 관련 호스트들이 IPsec을 구현해야된다.

## 장점
* end point간 암호화를 사용하기에 별도의 장비를 필요로하지 않는다.
* 터널링 모드에 비해 오버헤드의 감소로 장비의 효율이 증가한다.
## 단점
* ip 헤더의 원본은 보호하지 못하는 구조적 결함을 가진다.

## 터널(Tunnel Mode)
![tunnel](../../img/tunnel.png)

## 장점
* ip헤더까지 보호하므로 전송모드에 비해 보안적으로 안전하다.
* vpn을 사용할시에 유선망을 사용하는것보다 상대적으로 많이 저렴하면서 안전한 인프라를 구축할수있다. 

## 단점
* 별도의 라우터 장비를 필요로 한다.
* 라우터의 부담이 상대적으로 크다(오버헤드가 큼)

| Mode             	| Tunnel                                                                             	| Transport                                                                   	|
|------------------	|------------------------------------------------------------------------------------	|-----------------------------------------------------------------------------	|
| Description      	| Encrypts/authenticates the entire original IP packet and adds a new outer IP header | Encrypts/authenticates only the payload of the original IP packet           |
| Packet structure 	| Outer IP header (added by IPsec)                                                   	| Original IP header retained                                                 |
| Use case         	| Secure connection between two networks (e.g. site-to-site VPN)                     	| End-to-end communication between two devices                                |
| Security         	| More secure (protects original header)                                             	| Less secure (header exposed)                                               |
| Overhead         	| Higher                                                                             	| Lower                                                                       |
| Efficiency       	| Lower                                                                              	| Higher                                                                      |

## IKE(Inter Key Exchange)
* IPSec의 구성요소의 하나로 SA를 성립, 유지, 보수 하는데 필요한 데이터들을 안전하게 전달하기위해 사용한다.
* IPSec에서 두 컴퓨터 간의 `보안 연결 설정을 위해 사용되는 것`이다.
* 따라서 IKE 프로토콜은 SA를 협의하기위해 사용한다.
* IKE 프로토콜로 세션키를 교환한다.


## SA(Security Association)
* IPSec은 공개키 암호화 방식을 사용한다.(하이브리드 방식)
* 데이터 송수신간에 비밀데이터(인증되었거나, 암호화된 데이터)를 교환할 떄 사전에 암호 알고리즘, 키 교환방법, 키 교환 주기 등에 대한 합의가 이루워져야 한다.
* 데이터 교환 전에 통일되어야 할 이러한 요소들은 IPsec에서 SA로 정의함
* 하나의 SA는 단방향 데이터 전송에 적용되며 데이터 보호를 위해 보안 파라메터를 포함한다.

보안상 안전한 채널을 만들기 위한 SA는 양방향으로 통신하는 호스트 쌍에 여러개가 존재할수 있다.

# 핵심 개념
- IPsec은 AH/ESP, IKE, SA(보안 연합)로 구성된 프레임워크이며, 네트워크 계층에서 기밀성·무결성·인증을 제공한다.
- AH는 IP 헤더 포함 전체 패킷에 대한 무결성과 인증을 제공하지만 암호화는 제공하지 않는다.
- ESP는 페이로드(필요시 IP 헤더 일부)를 암호화하고 인증 옵션을 제공해 VPN에서 주로 사용된다.
- 터널 모드는 새 IP 헤더로 기존 패킷을 캡슐화해 사이트 간 보안 터널을 구성하며, 전송 모드는 종단 간 트래픽을 보호한다.
- IKEv2는 EAP, MOBIKE, 고속 재협상을 지원해 모바일/VPN 환경에서 표준으로 사용된다.

# 실무/시험 포인트
- IPSec 정책 구성 시 암호화·해시·DH 그룹의 "암호 스위트"가 양쪽 장비에서 일치해야 한다.
- Cisco IOS 예시: `crypto isakmp policy`, `crypto ipsec transform-set`, `crypto map` 순으로 정책을 정의한다.
- 리눅스 StrongSwan 예시: `/etc/ipsec.conf`, `/etc/ipsec.secrets`에서 IKEv2 프로파일과 SA를 설정 후 `ipsec statusall`로 확인한다.
- 사이트 간 VPN 장애 시 IKE Phase1/Phase2 상태(`show crypto ikev2 sa`, `ipsec status`)와 ACL/NAT 충돌을 점검한다.
- 클라우드 환경에서는 VTI(Virtual Tunnel Interface)나 BGP over IPsec을 함께 구성해 동적 라우팅을 유지한다.

## 예제: StrongSwan IKEv2 설정 스니펫
```ini
conn net-net
    left=203.0.113.1
    leftid=@hq
    leftsubnet=10.0.0.0/24
    right=198.51.100.2
    rightid=@branch
    rightsubnet=10.10.0.0/24
    ike=aes256-sha256-modp2048
    esp=aes256-sha256
    keyexchange=ikev2
    auto=start
```

## Mermaid: IPsec 흐름 요약
```mermaid
flowchart LR
    Client -->|Traffic| Gateway_A
    Gateway_A -->|IKE Phase1| Gateway_B
    Gateway_A -->|IKE Phase2 (ESP|AH)| Gateway_B
    subgraph Tunnel
        Gateway_A -->|Encrypted Payload| Gateway_B
    end
    Gateway_B --> InternalNet[Internal Network]
```

# TODO / 후속 연구
- [ ] AH/ESP 패킷 캡처 분석(`tcpdump`, Wireshark 스크린샷) 추가
- [ ] 클라우드(vWAN, AWS VPN) 구성 예제 링크 정리
- [ ] IKEv2 EAP-MSCHAPv2 인증 예제 및 보안 주의사항 추가

# 참고 자료
- [RFC 4301 – Security Architecture for the Internet Protocol](https://www.rfc-editor.org/rfc/rfc4301)
- [RFC 7296 – Internet Key Exchange Protocol Version 2 (IKEv2)](https://www.rfc-editor.org/rfc/rfc7296)
- [StrongSwan Documentation](https://docs.strongswan.org/)
