- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## bwapp
dvwa와 같은 모의해킹 훈련 템플릿이다.  
보통 bbox로 많이 접하는데 요즘엔 docker를 활용하여 빠르게 환경을 구축하기에  
bwapp을 도커로 올려서 테스트하기도 한다.  

``` bash
docker run --name bwapp -d -p 80:80 raesene/bwapp
```

uri를 localhost로 입력하게되면  
> Connection failed: Unknown database 'bWAPP'
와 같은 에러가 뜨는데

> localhost/install.php

를 눌러줘서 기본적으로 db를 설치해줘야된다.  


## login
id: bee
passowrd: bug

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
