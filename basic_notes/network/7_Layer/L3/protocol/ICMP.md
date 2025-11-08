- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## ICMP(Internet Control Message Protocol)
네트워크의 장치 간에 오류 메시지 또는 기타 정보를 전송하는 데 사용되는 프로토콜입니다.

## Desination Unreachable Error code table 
| Code 	| Description                               	|
|------	|-------------------------------------------	|
| 0    	| Network unreachable                       	|
| 1    	| Host unreachable                          	|
| 2    	| Protocol unreachable                      	|
| 3    	| Port unreachable                          	|
| 4    	| Fragmentation needed and DF set           	|
| 5    	| Source route failed                       	|
| 6    	| Destination network unknown               	|
| 7    	| Destination host unknown                  	|
| 8    	| Source host isolated                      	|
| 9    	| Network administratively prohibited       	|
| 10   	| Host administratively prohibited          	|
| 11   	| Network unreachable for TOS               	|
| 12   	| Host unreachable for TOS                  	|
| 13   	| Communication administratively prohibited 	|
| 14   	| Host Precedence Violation                 	|
| 15   	| Precedence cutoff in effect               	|


## 시간 경과 메시지(time-exceeded)
1. TTL 값이 0이 되면 데이터그램은 방문한 라우터에 의해 폐기된다. 그리고 시간경과(time exceeded) 메시지를 데이터그램이 폐기되었다는 것을 알리기 위해 근원지에게 전송된다.
2. 모든 데이터그램의 조각이 예상된 주기 시간에 도착하지 않았을떄 시간경과 메시지를 전송한다.

## 오류 보고 메시지
라우터나 호스트에서 IP 패킷을 전달할 떄 문제가 발생하였을 경우 사용한다.
1. 목적지 도달 불가 메시지(Destination Unreachable Message): 패킷이 중도에 폐기될 떄 사용하는 메시지  
2. 시간 초과 메시지(Time Exceeded Message): 패킷이 목적지에 전달되기 전에 시간 초과로 발생하는 메시지
3. 파라미터 문제 메시지(Parameter Problem Message): 패킷 헤더 문제 부분에 문제가 발생한 경우 사용되는 메시지
4. 재지정 메시지(Redirection Message): 동일 네트워크의 다른 라우터로 라우팅하는 것이 더 효율적이라고 판단되면 라우팅 경로를 변경하도록 ICMP 재지정 메시지를 호스트로 전송한다.

## ICMP Querty Message
네트워크 상황을 파악하기 위해 사용하며, 기본적으로 ICMP 오류 메시지와 동일한 포맷을 사용하지만 메시지 의존 필드를 식별자와 순서번호 필드로 나누어 사용한다.  
1. 에코 요청과 응답 메시지(Echo Request and Reply): 두 호스트 사이에 통신이 가능한지 검사할 때 사용되는 메시지
2. 타임 스탬프 요청과 응답 메시지(Timestamp Request and Reply): 패킷 전송시간 측정 메시지
3. 주소 마스크 요청과 응답 메시지(Address Mask Request Message): 호스트가 서브넷 마스크 값을 얻을 때 사용하는 메시지
4. 라우터 요청과 광고 메시지(Router Solicitation and Advertisement): 호스트가 네트워크 정보를 알고 싶을 떄 사용하는 메시지

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
