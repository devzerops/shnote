- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## 멀티스테이지(mult-stage)
* 이미지 크기를 줄이기위해 쓸데없는 아티팩트들을 줄이고 이미지 계층을 추가하는 기능
* 병렬로 stage를 실행하여 빌드속도를 향상시킬수있다.
    * 이미지들이 서로 종속적이면 빌드속도가 향상되지않는다
* dockerhub의 여러 이미지를 필요에따라 사용이 가능하며, 이미지의 용량까지 줄일수 있다.

## Example

## single-stage

```bash
docker build -t single:test1 -f Single_Stage_Dockerfile . 
```

## multi-stage
```bash
docker build -t multi:test .
docker run -p 8080:8080 multi:test
```

```bash
curl http://localhost:8080/ping
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
