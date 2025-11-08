- 마지막 업데이트: 2025-09-25
- 상태: 검토중

# 개요
Docker CLI는 컨테이너 실행(`docker run`), 라이프사이클 관리(`docker ps`, `docker stop`), 이미지/네트워크/볼륨 제어 명령으로 구성됩니다. 자주 사용하는 옵션을 정리해 두면 환경 구성 시간을 크게 단축할 수 있습니다.

## TL;DR
- 컨테이너 배포 점검 루틴: `docker run` → `docker ps --format` → `docker logs -f` → `docker exec`.
- 실행 결과와 리소스 확인은 `docker inspect`, `docker stats`, `docker system df`를 조합해 단일 스크립트로 자동화합니다.
- 재사용 가능한 환경을 위해 compose 템플릿을 함께 관리하고, 신뢰할 수 있는 명령 조합은 이 문서의 TODO에 테스트 로그로 남깁니다.

## docker run 주요 옵션
| 옵션 | 설명 | 실전 활용 |
|------|------|-----------|
| `-d`, `--detach` | 백그라운드 실행 | 데몬성 서비스 컨테이너 구동 |
| `-i`, `--interactive` / `-t`, `--tty` | 표준입력/TTY 유지 | `-it` 조합으로 셸 접속 |
| `--name` | 컨테이너 이름 지정 | 스크립트에서 컨테이너 식별 |
| `-p` | 포트 포워딩 `<host>:<container>` | `-p 8080:80` 등 서비스 노출 |
| `-v`, `--volume` | 호스트 ↔ 컨테이너 경로 마운트 | 데이터 영속·설정 공유 |
| `--env`, `-e` | 환경 변수 주입 | 비밀값/설정 전달 (`-e TZ=Asia/Seoul`) |
| `--restart` | 종료 시 재시작 정책(`no/always/on-failure/unless-stopped`) | 시스템 재부팅 후 서비스 유지 |
| `--privileged` / `--cap-add` | 커널 Capability 조정 | 저수준 네트워킹·디바이스 접근 |
| `--gpus` | NVIDIA GPU 자원 할당 | ML 워크로드 (`--gpus all`) |
| `--memory` / `--cpus` | 리소스 제한 | 과도한 자원 사용 방지 |

### 실행 예시
```bash
docker run -d \
  --name web \
  -p 8080:80 \
  -v $(pwd)/html:/usr/share/nginx/html:ro \
  --restart=unless-stopped \
  nginx:stable
```

### 실행 후 점검 루틴
```bash
# 컨테이너 목록 요약 출력
docker ps --format 'table {{.Names}}\t{{.Image}}\t{{.Status}}'

# 초기 오류 확인 (필요 시 tail)
docker logs -f web | head -n 20

# 환경 변수/볼륨 검증
docker inspect web --format '{{json .Config.Env}}' | jq .
docker inspect web --format '{{range .Mounts}}{{.Source}} -> {{.Destination}}\n{{end}}'
```

예상 출력 예시:
```
NAMES   IMAGE          STATUS
web     nginx:stable   Up 12 seconds

2025/09/25 10:02:41 [notice] 1#1: start worker processes
2025/09/25 10:02:41 [notice] 1#1: start worker process 30
```

## 기타 자주 쓰는 명령
| 명령 | 설명 |
|------|------|
| `docker ps -a` | 모든 컨테이너 조회 |
| `docker logs -f <name>` | 실시간 로그 확인 |
| `docker exec -it <name> /bin/bash` | 실행 중 컨테이너 셸 접속 |
| `docker cp <src> <container>:<dst>` | 파일 복사 |
| `docker image ls` / `docker image prune` | 이미지 목록·정리 |
| `docker network ls` / `docker network inspect` | 네트워크 정보 확인 |
| `docker stats` | 실시간 자원 사용량 모니터링 |
| `docker system df` | 이미지/컨테이너/볼륨 디스크 사용량 |

### Compose 전환 예제
```yaml
# docker compose.yaml (v2)
services:
  web:
    image: nginx:stable
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html:ro
    restart: unless-stopped
```

> `docker compose up -d`로 동일 구성을 재현하고, `docker compose logs -f web`으로 로그를 통합 확인합니다.

# 핵심 개념
- `docker run`은 이미지 풀 → 컨테이너 생성 → 실행까지 한 번에 수행하며, 동일 옵션으로 재생성하려면 compose 파일이나 스크립트화가 필요하다.
- Volume, Network, Restart 정책은 컨테이너 재구성 시에도 동일하게 재현할 수 있도록 관리해야 한다.
- 권한 관련 옵션은 최소 권한 원칙을 지켜 `--privileged` 대신 `--cap-add`, `--device` 등 세분화 사용을 우선 고려한다.

# 실무/시험 포인트
- 운영 환경에서는 `--restart=unless-stopped`와 헬스체크(`HEALTHCHECK`, `docker ps --format`)를 조합해 장애 감지에 대비한다.
- `docker stats`, `docker inspect`로 리소스 사용량과 설정을 확인하고, Compose/Kubernetes 이전 시 매핑 정보를 추출한다.
- 자격시험(DCA, CKAD 준비 등)에서는 `docker run` 옵션 조합 문제와 로그/exec 명령 사용이 빈출된다.

# TODO / 후속 연구
- [x] Compose v2로 동일 구성을 정의한 예시 추가.
- [ ] SELinux/AppArmor `--security-opt` 옵션별 차이 정리.
- [ ] 데몬 설정(`/etc/docker/daemon.json`) 관련 자주 쓰는 키 추가.
- [ ] `docker stats`/`docker system df` 샘플 출력 캡처 추가.

# 참고 자료
- Docker Docs – [CLI Reference](https://docs.docker.com/engine/reference/commandline/cli/).
- Docker Docs – [`docker run`](https://docs.docker.com/engine/reference/run/).
- Docker Docs – [Runtime options](https://docs.docker.com/engine/security/seccomp/).
