- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## container
``` bash
docker rm -f $(docker ps -aq)
```
> Only unused containers
``` bash
docker container prune
```

## image
``` bash
docker rmi $(docker images -q)
```

``` bash
docker image rm -f $(docker image ls -q)
```

> Only unused images
``` bash
docker image prune 
```

## volume

``` bash
docker volume prune
```

## all
> unused docker system(container, volume, images, network) delete
``` bash
docker system prune
```

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
