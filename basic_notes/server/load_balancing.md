- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## Load Balancing 이란?


## 기능
* Health Check
* 알고리즘에 따른 분산 처리
* NAT(Network Address Translation)
* DSR(Direct Server Return)


## Load Balancing 알고리즘

### Least Connection 알고리즘

* 현재 매핑되어 있는 커넥션이 가장 적은 서버로 세션을 연결해주는 방식
* 세션이 언제 끊길지(Keep alive)몰라서 문제가 생길수가 있다. 
![Least Connection 도식화](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb9StrT%2FbtrNZn7jWfb%2FkLXxMWMo60MNwovKNl7iik%2Fimg.png)


### Round Robin 알고리즘
* 들어오는 트래픽을 서버 순서대로 배치하는 방식
* 연결된 세션이 비교적 오래 사용되지 않는 경우에 채택하는 것이 좋다.
![Round Robin](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb6OTnO%2FbtrNYU5pBiE%2FGmLPIVBgVlc9uhnkYzKnJ0%2Fimg.png)

## Hash 알고리즘
이 알고리즘은 클라이언트 IP 주소나 URL 등을 해싱하여 그 값에 따라 각 서버에 분배하는 방식입니다.  
동일한 클라이언트의 요청이 항상 동일한 서버에 분배되기 때문에, 세션이 유지되어야 하는 경우에 유용합니다.
* 특정 기준을 잡아 특정 서버에 매핑하여 고정적으로 트래픽을 분산해주는 방식
* 일반적으로 사용되는 기준은 출발지(Client)의 IP가 된다.
![Hash](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FG8bGu%2FbtrNZGllf7A%2F01wpQyaRiCqS7vYf6t3FGK%2Fimg.png)


## 

### 


## Load Balancer
* 서버의 부하를 분산 해주는 장치 또는 기술을 뜻한다.
* 서버 상단 네트워크에 위치한다.
* 서버 한대에 집중되지 않게 트래픽을 관리하여 각 서버가 최적의 효율을 발휘할 수 있게 해준다.

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
