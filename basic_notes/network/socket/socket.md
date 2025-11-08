- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 소켓이란?
![socket](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FEgcag%2FbtrN33JiTAI%2FwAhz7vWIBSsZsu4hIWhYP1%2Fimg.png)

* 소켓은 파일이다.
    * 리눅스에서는 데이터 입출력 함수로도 다룰수있는 파일 그 자체다.<br> 따라서 파일 디스크립터로 파일과 똑같이 반환이 된다.
    * 윈도우는 파일핸들, 소캣핸들이 나뉘기에 완전히 파일로는 볼 수가 없다.

![Socket](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbz4YIj%2FbtrO6MYR0VT%2FLBAt4v2k4FfYxNJNstScZ1%2Fimg.png)

## 윈도우 소켓과 리눅스 소켓의 차이점

리눅스의 경우 파일 입출력 함수를 소켓 기반의 데이터 입출력에 사용할 수 있다. 반면 윈도우에서는 이것이 불가능하다.

이러한 차이가 생긴 이유는 윈도우시스템과 리눅스 시스템이 근본적으로 다르기때문인데 

## Linux 

내부적으로 소켓도 파일로 취급해서 파일이나 소켓이나 둘다 파일디스크립터가 반환된다.

## Window

윈도우에서는 파일 디스크립터를 `핸들`이라 표현한다.

파일 핸들, 소캣 핸들을 따로 구분한다. 따라서 리눅스처럼 완전히 동일하게 취급받진 않는다.

## 소켓 전송과정

![socket_process](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FblaHMW%2FbtqP10WPeJ6%2FzaY17OP44mj0BdOZdBTbXk%2Fimg.png)

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
