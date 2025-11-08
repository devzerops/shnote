- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
자꾸 까먹어서
``` bash
docker volume create portainer_data
```

``` bash
docker run -d -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```


출처: [portainer](https://docs.portainer.io/start/install/server/docker/linux)

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
