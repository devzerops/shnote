- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# 개요
트랜잭션(Transaction)은 데이터베이스 상태를 일관된 상태에서 다른 일관된 상태로 전환하는 논리적 작업 단위입니다. 병행 제어와 장애 복구의 기본 단위이기 때문에 시작/종료 시점을 명확히 정의해야 합니다.

## 상태 모델
| 상태 | 설명 |
|------|------|
| Active | 트랜잭션이 명령을 실행 중 |
| Partially Committed | 마지막 명령까지 실행, 로그 기록 완료 대기 |
| Committed | 커밋 완료, 변경 사항이 영구화 |
| Failed | 오류로 인해 더 진행할 수 없음 |
| Aborted | 롤백 수행 후 원래 상태로 복귀 |

# 핵심 개념
- **ACID 특성**: 원자성, 일관성, 격리성, 지속성은 트랜잭션 유효성 판단 기준이다.
- **로그 기반 회복**: WAL(Write-Ahead Logging)·UNDO/REDO 로그를 통해 장애 시점에서도 복구 가능.
- **격리 수준**: `READ UNCOMMITTED`, `READ COMMITTED`, `REPEATABLE READ`, `SERIALIZABLE`에 따라 Dirty/Non-repeatable/Phantom Read 발생 가능성이 달라진다.
- **동시성 제어 기법**: 2PL, 타임스탬프 오더링, MVCC 등으로 상호 배제와 직렬 가능성 확보.
- **커밋/롤백**: COMMIT은 로그에 기록된 변경을 확정하고, ROLLBACK은 이전 일관된 상태로 복원한다.

# 실무/시험 포인트
- 은행 잔액 이체, 주문-결제 처리 등은 단일 트랜잭션으로 묶어야 데이터 불일치를 방지할 수 있다.
- 장애 대비를 위해 주기적 체크포인트, 트랜잭션 로그 백업 전략을 설계한다.
- DBMS별 기본 격리 수준(PostgreSQL: READ COMMITTED, MySQL InnoDB: REPEATABLE READ 등)을 기억하면 튜닝 시 도움이 된다.
- 시험에서는 상태 전이, 격리 수준별 부작용, COMMIT/ROLLBACK 동작을 묻는 문제가 자주 나온다.

# TODO / 후속 연구
- 각 격리 수준에서 발생하는 현상 사례 표 작성.
- MVCC 구현(DB별 Undo Segment, Snapshot 등) 비교 문서로 확장.
- 분산 트랜잭션 2PC/3PC, Saga 패턴과 연계 자료 링크 추가.

# 참고 자료
- Gray & Reuter, *Transaction Processing: Concepts and Techniques*.
- PostgreSQL Documentation – Transaction Isolation, MVCC.
- Oracle Concepts – Undo/Redo 로그 구조.
