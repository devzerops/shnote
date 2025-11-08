- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
* `Receive Window`  : 수신자(Receiver)가 송신자(Sender)에게 알려 주는 현재 수신 가능한 비어 있는 Rx Socket Buffer 크기

* `Send Window (Tx Window)` : 송신자가 수신자의 ACK 없이 한번에 전송할 수 있는 데이터 양

* `Window Scaling` :  2bytes로 고정된 TCP header의 Window Size 제약을 극복하기 위해서 사용하는 TCP option

* `Sliding Window` :  송신자가 수신자의 Rx socket Buffer overflow가 발생되지 않도록 전송량을 control하는 매카니즘

* `Window Full` : Wireshark가 보여주는 분석 정보의 하나로 송신자가 수신자 ACK 없이 보낼 수 있는 데이터를 모두 보냈음을 의미함.

* `Zero Window` : 수신자의 Rx Socket buffer가 꽉 찰 것 같은 상황이 닥치면 수신자는 Window Size를 0으로 채워 보냄.

* `Congestion Window` : Network에서 congestion 발생시 network 부하를 감소시키기 위해서 사용하는 송신자의 Window

* `Window based flow control` : 수신자의 Rx Socket Buffer overflow가 발생되지 않도록 송신자의 전송량을 control하는 매카니즘

* `Window based congestion control` : 송신자와 수신자 사이의 네트워크에서 Congestion을 회피하기 위해서 사용하는 매카니즘

* `buffer overflow` : buffer에 저장공간이 꽉차서 새로이 도착하는 데이터가 버려지는 것을 buffer overflow가 발생한다고 이야기 합니다.

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
