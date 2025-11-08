- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## IGMP(Internet Group Management Protocol)

인터넷 프로토콜 스위트(IP 스위트)의 일부로, 멀티캐스트 그룹 관리를 위해 설계된 프로토콜입니다.  
IGMP는 호스트가 속한 네트워크에 존재하는 멀티캐스트 그룹에 가입하거나, 그룹에서 나가는 등 멀티캐스트 트래픽을 관리합니다.

IGMP는 호스트와 라우터 간의 통신을 위해 사용됩니다.  
호스트는 IGMP를 사용하여 해당 호스트가 속한 네트워크에서 어떤 멀티캐스트 그룹에 가입할지, 라우터는 해당 네트워크로 들어오는 멀티캐스트 패킷을 어떤 호스트에게 전달해야 하는지를 판단하기 위해 IGMP를 사용합니다.

IGMP는 인터넷 계층(OSI 모델의 3계층)에서 동작합니다. IGMP는 호스트와 라우터 간의 통신을 위해 사용되기 때문에, 네트워크 계층에서는 IP 프로토콜과 함께 동작합니다.  
따라서, IGMP는 OSI 모델에서 3계층에 해당합니다.

## IGMP flag

| IGMP 플래그 | 의미                                  |
|-------------|---------------------------------------|
| 0x01        | IGMP Membership Query                 |
| 0x11        | IGMP Membership Report                |
| 0x16        | IGMPv2 Membership Report (Deprecated) |
| 0x17        | IGMPv2 Leave Group                    |
| 0x22        | IGMPv3 Membership Report              |
| 0x30        | Router Alert                          |

## IGMP header field

| IGMP 헤더 필드          | 크기 (바이트) | 의미                                                             |
|-------------------------|---------------|------------------------------------------------------------------|
| Type                    | 1             | IGMP 플래그                                                      |
| Max Response Time       | 1             | IGMP Membership Query에서 사용됨                                 |
| Checksum                | 2             | 헤더 체크섬                                                      |
| Group Address           | 4             | 멀티캐스트 그룹 주소 (Membership Report, Leave Group에서 사용됨) |
| Reserved                | 2             | 0으로 설정되어야 함                                              |
| Number of Group Records | 2             | 멀티캐스트 그룹 레코드 개수 (IGMPv3에서 사용됨)                  |
| Group Record            | 가변          | 멀티캐스트 그룹 레코드 (IGMPv3에서 사용됨)                       |

## 사용예시
IGMP의 활용 예시로는 IPTV(Internet Protocol Television)가 있습니다. IPTV는 인터넷 프로토콜을 사용하여 TV 방송을 제공하는 서비스로, 하나의 TV 방송을 다수의 이용자에게 동시에 전송할 때 멀티캐스트 방식을 사용합니다. 이때, IPTV에서는 IGMP 프로토콜을 이용하여 각 이용자가 참여하는 멀티캐스트 그룹을 관리하고, 각 이용자가 원하는 채널을 선택하여 시청할 수 있습니다. 이를 통해 IPTV 서비스 제공자는 대역폭 사용량을 최적화하면서도 이용자가 원하는 채널을 안정적으로 제공할 수 있습니다.

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
