- 마지막 업데이트: 2025-09-24
- 상태: 검토중

# ACID 속성 개요
ACID는 트랜잭션이 데이터베이스 일관성을 유지하기 위해 만족해야 하는 네 가지 성질을 의미합니다. 금융, 결제, 주문 등 상태 변경이 중요한 시스템에서 핵심 검증 항목으로 다뤄집니다.

# 핵심 개념
- **Atomicity (원자성)**: 트랜잭션은 전부 성공하거나 전부 실패해야 하며, 실패 시 모든 작업을 롤백합니다.
- **Consistency (일관성)**: 트랜잭션 전후에 데이터는 정의된 규칙(무결성 제약, 트리거 등)을 만족해야 합니다.
- **Isolation (격리성)**: 동시에 실행되는 트랜잭션이 서로 간섭하지 않도록 가시성과 잠금 수준을 관리해야 합니다.
- **Durability (지속성)**: 커밋된 결과는 전원 장애 등에도 영속 저장소(로그, WAL)에 의해 보존되어야 합니다.

# 실무/시험 포인트
- WAL(Write-Ahead Logging), 체크포인트 등 영속성 메커니즘 구현을 이해해야 복구 시나리오를 설계할 수 있습니다.
- Isolation 수준(`READ COMMITTED`, `SERIALIZABLE` 등)에 따라 `Dirty Read`, `Phantom Read` 발생 가능성이 달라집니다.
- 분산 트랜잭션에서는 2PC/3PC, Saga 패턴 등 보완 기법을 추가적으로 고려해야 합니다.

# TODO / 후속 연구
- Isolation 수준별 현상(Dirty/Non-repeatable/Phantom Read) 표 작성.
- PostgreSQL, MySQL, Oracle 기본 Isolation 수준 비교.
- 분산 트랜잭션 CAP 정리 문서와 상호 링크.

# 참고 자료
- [PostgreSQL Documentation: Transaction Isolation](https://www.postgresql.org/docs/current/transaction-iso.html)
- [Oracle Database Concepts](https://docs.oracle.com/en/database/) – 트랜잭션 관리 장.
- [Martin Kleppmann, *Designing Data-Intensive Applications*] – 분산 시스템에서의 트랜잭션 설계.

# 개요
- (정리 예정)
