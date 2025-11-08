- 마지막 업데이트: 2025-09-24
- 상태: 초안

# 개요
## docker volume
파일을 저장할 형식, 위치, 설정 등을 저장하여 docker container을 효과적으로 관리하기 위해 만든 하나의 형태의 시스템입니다.  
단순하게 사용해도 파일을 관리하기 훨씬 쉬워지기에 유용합니다.  
  
한가지 알아야할점은 default 볼륨은 binding이라는 시스템을 사용합니다.  
이 바인딩 시스템 또한 훌륭한 볼륨 시스템이지만 오버헤드가 자주발생하여 다른 시스템에 비해 느려집니다.  
이러한 차이를 파악하고 서비스에 유형에 맞게 적절한 볼륨을 만들어서 효율적인 서비스를 만들어보세요.  


``` bash
docker volume create <name>
```

``` bash
docker volume ls
```

``` bash
docker volume inspect <name>
```

안쓰는 볼륨 지우기  
``` bash
docker volume prune
```

위 명령어와 아래 예시를 통하여 도커 볼륨의 개념을 익혀봅시다.  
``` bash
docker volume create bwapp && \
docker run -d --name bwapp -p80:80 -v bwapp:/var/log/ raesene/bwapp:latest && \
docker ps -a --format "table {{.Ports}} {{.Name}}"
```


## volume basic location change

``` bash
docker volume create -d local -o o=bind -o type=none -o device=/home/user/data my-volume
```

``` bash
docker run --mount source=my-volume,target=/data <image-name>
```

## volume option example

* device: 
* type: 볼륨을 저장할 유형
* o: 볼륨을 바인딩으로 저장할기 tmpfs와 같은것으로 저장할지 나타내는 유형 (default는 bind)

``` bash
--opt type=none
--opt device=/home/db/postgresql
--opt o=bind
```

## --opt에 대한 설명

|    Option   |                                   Description                                   |         Example        |
|:-----------:|:-------------------------------------------------------------------------------:|:----------------------:|
| type=bind   | Specifies that the volume should be a bind mount. This is the default option.   | --opt type=bind        |
| type=volume | Specifies that the volume should be a named volume.                             | --opt type=volume      |
| type=tmpfs  | Specifies that the volume should be a tmpfs mount.                              | --opt type=tmpfs       |
| device      | Specifies the path on the host to the file or directory that should be mounted. | --opt device=/host/dir |
| o           | Specifies mount options for the bind mount.                                     | --opt o=bind           |
| o=bind      | Specifies that the volume should be a bind mount with the default options.      | --opt o=bind           |
| o=volume    | Specifies that the volume should be a named volume with the default options.    | --opt o=volume         |
| o=tmpfs     | Specifies that the volume should be a tmpfs mount with the default options.     | --opt o=tmpfs          |


[docker volume DOC](https://docs.docker.com/storage/volumes/)

# 핵심 개념
- (정리 예정)

# 실무/시험 포인트
- (정리 예정)

# TODO / 후속 연구
- (정리 예정)

# 참고 자료
- (추가 예정)
