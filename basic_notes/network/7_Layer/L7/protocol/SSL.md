- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## SSL protocol Stack
![protocol Stack](https://img.brainkart.com/imagebk9/ZZjtoMH.jpg)


| 프로토콜 | 설명                |
| -------- | -----------------  | 
| Handshake| * Record 프로토콜에 대한 보안 매개변수를 제공 <br> * 암호 집합을 설정하고 키와 보안 매개변수를 제공한다. <br> * 클라이언트가 서버에 대해 그리고 서버가 클라이언트에 대해 인증된다(상호인증)|
| Record   | * 응용 계층으로부터 오는 데이터 뿐만 아니라 TLS의 상위 프로토콜로부터 오는 메시지를 전송 <br> * Record 프로토콜에서 오는 메시지는 보통 TCP인 전송 계층의 페이로드이다. |
| ChangeCipherSpec | * 암호학적 비밀을 보내는데 사용 |
| Alert    | 비정상 조건을 알리는데 사용 |
| Heartbeat| 프로토콜 개체의 가용성을 모니터링 할 때 사용|

## SSL Hanshake protocol
![ssl handshake](https://img.brainkart.com/imagebk9/QXH36wj.jpg)

| 메시지 유형 | 매개변수 |
| ----------- | ------- |
| hello_request | null |
| client_hello  | version, random, session id, ciphersuite, compression method|
| server_hello  | version, random, session id, ciphersuite, compression method|
| certificate   | 연속된 X 509v3 인증서|
| server_key_exchange| parameters, signature|
| certificate_request| type, authorities|
| server_done   | NULL |
| certificate_verify | Signature |
| client_key_exchange| parameters, signature|
| finished      | hash value |

봐야할거: https://www.brainkart.com/article/Secure-Socket-Layer-and-Transport-Layer-Security_8480/

## SSL 취약점

SSL Stripping Attack: 공격자가 중간에서 SSL로 보류되는 세션을 벗겨냄
Heartbleed Attack: SSL이 페이로드 값을 검증하지않앗습니다.

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
