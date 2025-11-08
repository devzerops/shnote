- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 프로세스 개요
프로세스(process)는 실행 중인 프로그램으로, 운영체제가 CPU와 메모리 등 자원을 할당하고 스케줄링하는 기본 단위입니다. 하드디스크에 저장된 프로그램이 메모리에 적재되어 PCB(Process Control Block)를 통해 관리되는 순간 프로세스로 간주됩니다.

# 핵심 개념
- **구성 요소**: 실행 코드, 데이터/스택/힙 메모리 영역, PCB(프로세스 상태·우선순위·CPU 레지스터·메모리 맵 등).
- **생성 흐름**: 프로세스 생성 → 메모리 공간 확보 → PCB 초기화 → 스케줄러가 준비 큐에 추가.
- **상태 전이**  
  - `Ready`: CPU 할당을 기다리는 상태.  
  - `Running`: CPU를 점유하여 명령을 실행 중인 상태.  
  - `Blocked(Waiting)`: I/O 등 이벤트를 기다리는 상태.  
  - `Terminated`: 실행이 종료되어 자원을 반납한 상태.  
  필요 시 `New`, `Suspended`, `Zombie` 등을 추가로 구분합니다.
- **상태 전이 이벤트 용어**:  
  - `Dispatch`: 준비 큐에서 선택된 프로세스에 CPU를 할당.  
  - `Preemption`: 타이머 만료나 우선순위 변화로 CPU를 회수.  
  - `Wake-up`: 대기 상태 프로세스가 이벤트 완료 후 준비 큐로 이동.  
  - `Spooling`: 입출력을 디스크 등에 임시 저장해 순차 처리.

# 실무/시험 포인트
- PCB는 문맥 교환 시 반드시 보존해야 하는 정보로, 커널 주소 공간에 위치합니다.
- 멀티프로그램 환경에서 CPU 이용률을 높이려면 준비 큐 관리(우선순위, 타임 슬라이스)가 중요합니다.
- 프로세스-스레드 구성을 설계할 때 공유 자원 보호(뮤텍스/세마포어) 전략을 미리 정의해야 합니다.
- 상태 전이 다이어그램과 용어(`Dispatch`, `Interrupt`, `I/O completion`)를 시험에서 자주 묻습니다.

# TODO / 후속 연구
- 프로세스 생성 API(`fork`, `exec`, `CreateProcess`) 비교 표 추가.
- `OS_notes/scheduling.md`와 연결되는 스케줄링 알고리즘 요약 링크 삽입.
- PCB 구조 예시 그림 작성 및 `drawio/` 폴더에 도식화 저장.

# 참고 자료
- [Operating System Concepts (Silberschatz 등)](https://www.os-book.com/) – 3장 프로세스 관리.
- [Linux man pages: `fork(2)`, `execve(2)`](https://man7.org/linux/man-pages/) – POSIX 프로세스 생성 호출.
