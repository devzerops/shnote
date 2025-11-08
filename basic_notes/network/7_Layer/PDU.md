- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## PDU란?
* Protocol Data Unit
![PDU](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8d/OSI_Model_v1.svg/870px-OSI_Model_v1.svg.png)

## Stream이란?

* 언제 `끝날줄 모르는 데이터의 흐름`을 말한다.
* Socket수준에서 Stream data를 생성한다.

## Segment이란?
* 소켓에서 kernel이 다룰수있도록 `Segmentation`을 시킨것
* TCP/UDP에서 다룰수있는 데이터로 `Stream을 자른것`
* 세그먼트의 단위는  MTU(Maximum Transport Unit)에 기반해서 MSS(Maximum Segment Size)값에 따라 자름

## packet이란?
* Segment를 인터넷환경에서 전송가능한 형태로 자른것
* MTU(Maximum Transmission Unit)

## Frame이란?
* packet을 실어서 나르는 단위

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
