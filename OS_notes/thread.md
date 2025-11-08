- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 스레드 개요
스레드(thread)는 프로세스 내부에서 실행 흐름을 담당하는 최소 단위입니다. 하나의 프로세스는 여러 스레드를 포함할 수 있고, 스레드는 코드/데이터/힙 등의 자원을 공유하며 스택과 레지스터 집합은 독립적으로 가집니다.

# 핵심 개념
- **장점**: 경량 문맥 교환, 메모리 공유, 병행성과 다중 CPU 활용 향상, 응답성 개선.
- **위험 요소**: 동기화 실패에 따른 경쟁 조건, 교착 상태, 컨텍스트 스위칭 빈도 증가 시 오버헤드.
- **스레드 모델**  
  - 사용자 수준 스레드(1:N): 라이브러리가 스케줄링, 문맥 교환 비용 적음, 커널 블로킹 시 전체 대기.  
  - 커널 수준 스레드(1:1): 커널이 직접 관리, 다중 CPU 활용, 문맥 교환 비용 상대적으로 큼.  
  - 하이브리드(M:N): 사용자 스레드를 여러 커널 스레드에 매핑해 장점을 조합.
- **스케줄링 연계**: 스레드는 커널 또는 런타임 스케줄러의 우선순위 정책에 따라 실행 순서가 결정됩니다.

# 실무/시험 포인트
- 공유 자원 보호를 위해 뮤텍스, 세마포어, 조건 변수 등 동기화 원리를 숙지해야 합니다.
- CPU 바운드/IO 바운드 작업을 분리해 스레드와 프로세스 배치를 설계하면 성능 최적화가 가능합니다.
- POSIX `pthread`, Windows `_beginthreadex`, Java `ExecutorService` 등 구현별 API 차이를 비교하세요.
- 멀티스레드 환경에서는 캐시 일관성과 메모리 장벽 개념도 중요합니다.

# TODO / 후속 연구
- 사용자 수준 스레드 라이브러리 사례(Green Threads, Goroutines) 정리.
- 컨텍스트 스위칭 비용을 측정한 벤치마크 표 추가.
- 동기화 실패 사례를 `mutex.md`, `semaphore.md` 문서와 상호 링크.

# 참고 자료
- [Operating System Concepts](https://www.os-book.com/) – 4장 스레드와 병행성.
- [POSIX Threads Programming](https://computing.llnl.gov/tutorials/pthreads/) – LLNL 튜토리얼.
- [Microsoft Docs: Windows Threads](https://learn.microsoft.com/windows/win32/procthread/about-threads) – 커널 스레드 모델.
