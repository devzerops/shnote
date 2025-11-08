- 마지막 업데이트: 2025-09-26
- 상태: 초안

# 개요
Docker Compose는 복수의 컨테이너 서비스를 하나의 정의 파일(`compose.yaml`)로 선언하고 실행 순서·환경변수·네트워크를 일괄 제어하는 도구입니다. 로컬 개발과 경량 서비스 오케스트레이션에 적합하며, Docker CLI와 동일한 인증 컨텍스트를 공유합니다.

## 첫 실행 절차
1. **환경 변수 준비**: `.env` 파일을 저장소 루트 또는 Compose 파일과 동일한 디렉터리에 배치합니다.
   ```env
   APP_IMAGE=ghcr.io/example/app:1.2.0
   APP_PORT=8080
   DB_PASSWORD_FILE=./secrets/db_pass.txt
   ```
2. **서비스 정의 예시** (`compose.yaml`):
   ```yaml
   services:
     app:
       image: ${APP_IMAGE}
       ports:
         - "${APP_PORT}:8080"
       env_file:
         - ./app.env
       depends_on:
         db:
           condition: service_healthy
       healthcheck:
         test: ["CMD", "curl", "-f", "http://localhost:8080/health"]
         interval: 30s
         timeout: 5s
         retries: 5
     db:
       image: postgres:16
       environment:
         POSTGRES_PASSWORD_FILE: ${DB_PASSWORD_FILE}
       volumes:
         - pgdata:/var/lib/postgresql/data
       healthcheck:
         test: ["CMD-SHELL", "pg_isready -U postgres"]
         interval: 30s
         timeout: 5s
         retries: 5
   volumes:
     pgdata:
   ```
3. **실행**:
   ```bash
   docker compose up -d --wait
   docker compose ps
   ```
   - `--wait` 옵션은 모든 서비스의 헬스체크가 성공할 때까지 CLI를 반환하지 않습니다.

## 운영 점검 커맨드
- `docker compose ls --format table`로 프로젝트별 상태를 목록화합니다.
- `docker compose logs -f --since 10m app`으로 서비스 로그를 추적합니다.
- `docker compose events --since 1h`를 활용해 재시작/오류 이벤트를 빠르게 수집합니다.
- `docker inspect $(docker compose ps -q app)`으로 자원 제한(CPU/메모리) 적용 여부를 검증합니다.

### 샘플 출력
```text
$ docker compose ls --format table
NAME      STATUS              CONFIG FILES
notes     running(2)          /opt/stacks/notes/compose.yaml

$ docker compose ps
NAME                IMAGE                         COMMAND                  SERVICE   CREATED              STATUS              PORTS
notes-app-1         ghcr.io/example/app:1.2.0     "/entrypoint.sh"        app       2 hours ago          Up 2 hours          0.0.0.0:8080->8080/tcp
notes-db-1          postgres:16                   "docker-entrypoint.s"   db        2 hours ago          Up 2 hours (healthy) 5432/tcp

$ docker compose logs --since 10m app | tail -n 4
app-1  | 2025-09-26T09:27:04Z  INFO  http  Request completed GET /health 200 2ms
app-1  | 2025-09-26T09:29:17Z  WARN  worker  Retry queue backlog=12
app-1  | 2025-09-26T09:30:01Z  INFO  metrics  Prometheus scrape ok
app-1  | 2025-09-26T09:31:22Z  INFO  http  Request completed POST /api/login 200 28ms

$ docker compose events --since 1h --until now | grep -E "health_status|restart"
2025-09-26T08:55:31Z service=app container=notes-app-1 event=health_status: healthy
2025-09-26T08:56:47Z service=db  container=notes-db-1  event=health_status: healthy
```

## 로그 보관 템플릿
```bash
#!/usr/bin/env bash
set -euo pipefail
STACK_NAME="notes"
DEST="/var/log/docker/${STACK_NAME}"
mkdir -p "$DEST"

# 상태 스냅샷
{
  date --iso-8601=seconds
  docker compose ps
} >"$DEST/$(date +%Y%m%dT%H%M%S)-ps.txt"

# 최근 30분 로그
docker compose logs --since 30m >"$DEST/$(date +%Y%m%dT%H%M%S)-logs.txt"
```
- 로그 파일은 Git 외부 경로에 저장하고, 필요 시 `gzip`으로 압축 후 SHA-256 해시를 남깁니다.

## 점검 체크리스트
- [ ] `.env`와 시크릿 파일을 `gitignore`에 포함했는지 확인.
- [ ] `depends_on` 조건 대신 헬스 체크로 서비스 의존성을 선언.
- [ ] Compose 버전(`docker compose version`)이 prod 환경과 일치하는지 확인.
- [ ] Build 캐시 재사용이 필요하면 `docker buildx bake`와의 연계를 검토.

## TODO / 후속 연구
- [ ] `docker compose cp` 활용 사례 추가.
- [ ] 프로필 기반 배포(`--profile`) 시나리오 정리.
- [ ] Compose 파일을 Swarm/ACI로 변환(`docker compose convert`)할 때의 제약 조사.

## 참고 자료
- Docker Docs – <https://docs.docker.com/compose/>
- Compose Specification – <https://github.com/compose-spec/compose-spec>
- Docker Captains Blog – *Healthcheck patterns for Compose*.
