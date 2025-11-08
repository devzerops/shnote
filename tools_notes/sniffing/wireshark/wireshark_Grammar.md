- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 화면 표시 필터 비교 연산자

| English 	| Alias 	| C-like 	| Description 	| Example 	|
|---	|---	|---	|---	|---	|
| eq 	| any_eq 	| == 	| Equal (any if more than one) 	| ip.src == 10.0.0.5 	|
| ne 	| all_ne 	| != 	| Not equal (all if more than one) 	| ip.src != 10.0.0.5 	|
|   	| all_eq 	| === 	| Equal (all if more than one) 	| ip.src === 10.0.0.5 	|
|   	| any_ne 	| !== 	| Not equal (any if more than one) 	| ip.src !== 10.0.0.5 	|
| gt 	|   	| > 	| Greater than 	| frame.len > 10 	|
| lt 	|   	| < 	| Less than 	| frame.len < 128 	|
| ge 	|   	| >= 	| Greater than or equal to 	| frame.len ge 0x100 	|
| le 	|   	| <= 	| Less than or equal to 	| frame.len <= 0x20 	|
| contains 	|   	|   	| Protocol, field or slice contains a value 	| sip.To contains "a1762" 	|
| matches 	|   	| ~ 	| Protocol or text field matches a Perl-compatible regular expression 	| http.host matches "acme\\.(org\|com\|net)" 	|

## 조건 연산자

| English 	| C-like 	| Description 	| Example 	|
|---	|---	|---	|---	|
| and 	| && 	| Logical AND 	| ip.src==10.0.0.5 and tcp.flags.fin 	|
| or 	| \|\| 	| Logical OR 	| ip.src==10.0.0.5 or ip.src==192.1.1.1 	|
| xor 	| ^^ 	| Logical XOR 	| tr.dst[0:3] == 0.6.29 xor tr.src[0:3] == 0.6.29 	|
| not 	| ! 	| Logical NOT 	| not llc 	|
| […] 	|   	| Subsequence 	| See “Slice Operator” below. 	|
| in 	|   	| Set Membership 	| http.request.method in {"HEAD", "GET"}. See “Membership Operator” below. 	|


## 알고리즘 연산자

| Name 	| Syntax 	| Description 	|
|---	|---	|---	|
| Unary minus 	| -A 	| Negation of A 	|
| Addition 	| A + B 	| Add B to A 	|
| Subtraction 	| A - B 	| Subtract B from A 	|
| Multiplication 	| A * B 	| Multiply A times B 	|
| Division 	| A / B 	| Divide A by B 	|
| Modulo 	| A % B 	| Remainder of A divided by B 	|
| Bitwise AND 	| A & B 	| Bitwise AND of A and B 	|

## 조건 필터 연산자

| Function 	| Description 	|
|----------	|---	|
| upper 	| Converts a string field to uppercase. 	|
| lower 	| Converts a string field to lowercase. 	|
| len 	    | Returns the byte length of a string or bytes field. 	|
| count 	| Returns the number of field occurrences in a frame. 	|
| string 	| Converts a non-string field to a string. 	|
| max 	    | Return the maximum value for the arguments. 	|
| min 	    | Return the minimum value for the arguments. 	|
| abs 	    | Return the absolute value for the argument. 	|

## 자주 사용하는 구문

ip.addr == 192.168.2.1 
출발지나 목적지의 ip가 192.168.2.1일경우 출력

ip.addr == 192.168.2.1 && ip.addr ==192.168.2.2 두 개의 정의된 ip 주소를 모두 출력

http or arp 모든 http와 dns 프로토콜을 출력

tcp.port == 4000 출발지나 목적지의 포트가 4000인 tcp 패킷을 출력

tcp.flags.reset == 1 모든 TCP reset 플래그가 활성화된 패킷을 출력

http.request 모든 http get 요청 패킷을 출력

tcp contains traffic traffic이라는 단어를 포함하는 tcp 패킷을 출력

!(arp or icmp or dns) 괄호 내용을 모두 제외한 패킷을 출력

contains 33:27:58 핵스값(0x33 0x27 0x58) 필터

tcp.analvsis.retransmission 추적에서 모든 전송을 표시


출처: https://www.wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
