- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 기본 네트워크 구조 
## NAT Mode

* default mode
* DNAT으로 outbound를 master os의 ip가 된다는 특징을 가진 네트워크 기술
* SNAT으로 master os와 동일시하게 만들순있으나 보안상 취약하기에 사용하는것을 추천하지않음

## Host Only mode

* 외부에 접근불가 모드
* 네트워크가 외부와 통신이 가능하지않다. 
* 완전한 센드박스형 모드이다. 보안상 문제로 host Only 모드를 container 내부의 시스템을 구축할때 사용을 하기도한다.  

## 브릿지 모드

* 컨테이너를 도커 네트워크 인터페이스(docker0) 로 설정
* 컨테이너는 docker_proxy라는 데몬을 통하여 호스트와 연결 
* 컨테이너는 기본적으로 호스트와 격리된 상태

## 도커 컨테이너 모드

* bridge: This is the default network mode in Docker. It creates a virtual network interface on the host, and each container is connected to this network via a virtual Ethernet interface.

* none: This mode disconnects the container from any network. It can be used when the container does not require network connectivity.

* overlay: This mode allows you to connect multiple Docker daemons together and create a virtual network that spans across hosts. It enables multi-host networking for Docker containers.

* macvlan: This mode creates a new layer 2 network on the host, and assigns a unique MAC address to each container. This allows the container to have a directly-assigned IP address on the host's network and is useful for running containers on a LAN with DHCP.
 
* host: This mode connects the container directly to the host's network stack, giving the container access to the host's network interfaces and IP addresses.

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
