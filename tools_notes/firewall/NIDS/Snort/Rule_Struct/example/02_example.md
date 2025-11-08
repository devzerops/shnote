- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 예제 2

여기까지만 설명 추가하고 나머지는 기능만 설명

``` snort
alert udp $HOME_NET any -> any 53 (msg:"ET POLICY Dropbox DNS Lookup - Possible
Offsite File Backup in Use"; content:"|01 00 00 01 00 00 00 00 00 00|"; depth:10;
offset:2; content:"|09|client-lb|07|dropbox|03|com|00|"; nocase; distance:0;
fast_pattern; reference:url,dropbox.com; classtype:policy-violation; sid:2020565;
rev:1; metadata:created_at 2015_02_24, updated_at 2015_02_24;)
```

## alert 
``` snort
alert udp $HOME_NET any -> any 53
```

인아웃바운드의 udp를 사용하는 53번 포트 즉 DNS를 찾으라는 의미 
any는 8.8.8.8 일수도 있고 8.8.4.4 일 수도 있고 사내 dns일 수도 있기떄문이다.

## msg
``` snort
(msg:"ET POLICY Dropbox DNS Lookup - Possible Offsite File Backup in Use";
```

msg 는 alert가 발동하면 입력하는 메시지

## content
``` snort
content:"|01 00 00 01 00 00 00 00 00 00|"; depth:10; offset:2;
```

`offset`: 페이지의 시작을 알리는 옵션
`depth`: 페이지의 끝을 알리는 옵션

검사할 패킷 01 00 00 01 00 00 00 00 00 00 을 찾는데

offset:2 (2번째 바이트) 부터 -> depth:10 (10번쨰 바이트)까지 찾으라는 의미

``` snort
content:"|09|client-lb|07|dropbox|03|com|00|"; nocase; distance:0;
```

`nocase`: 대소문자 구분하지 않음


## fast_pattern
``` snort
fast_pattern;
```

`fast_pattern`: content의 길이가 가장 짧은 것 부터 찾아라

## refernce
``` snort
reference:url,dropbox.com;
```

`refernce`: 어떤것을 참조했냐 나타내는 구문

url인 dropbox.com을 참조했다는것을 나타냄 (주석하고 비슷함)

## classtype

``` snort
classtype:policy-violation; sid:2020565; rev:1; metadata:created_at 2015_02_24, updated_at 2015_02_24;)
```

`classtype`: 탐지된 룰이 어느 타입에 속하는지 의미함

config classification: (분류), (설명), (우선순위)

`sid`: Signature ID 를 뜻함

0 ~ 1000000: SourceFire VRT에서 받아오는 시그니처에 할당된 SID 

2000000 ~ 2999999: Emerging Threats에섭 ㅏㄷ아오는 시그니처에 할당된 sid값

3000000 ~: 원하는 대로 사용 가능한 영역


`rev`: 룰의 버전을 나타냄 sid가 같은 룰이 존재할경우 snort에서 자동으로 rev가 높은것을 선택 <br> 이렇게 하는 이유는 룰이 자동으로 추가되는데 기존 룰을 삭제하거나 새로운 룰에 sid를 다르게 매기기엔 어려운 상황이 많아서 그렇다.


`metadata`:룰을 만든 날짜와 수정한 날짜

2015_02_24년에 만들어졌고 2015_02_24년도에 업데이트 됐다.

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
