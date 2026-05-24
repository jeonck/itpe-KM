---
title: 트랜잭션 및 동시성 제어
weight: 2
---

트랜잭션은 데이터베이스의 논리적 작업 단위로, ACID 특성을 통해 데이터의 일관성과 무결성을 보장한다. 다중 사용자 환경에서 동시에 발생하는 트랜잭션 간의 충돌을 제어하고, 시스템 장애 발생 시 데이터를 복원하는 메커니즘은 DBMS의 핵심 기능이다. 기술사 시험에서 ACID, 동시성 문제, 고립 수준 이상 현상 매트릭스, 회복 기법은 매년 단골 출제 영역이다.

| 순서 | 토픽 | 핵심 키워드 | 중요도 |
|:---:|:---|:---|:---:|
| 1 | [트랜잭션](transaction) | ACID, 상태 전이도, Commit·Rollback, 원자성·일관성·고립성·영속성 | ★★★★★ |
| 2 | [동시성 제어](concurrency-control) | Lost Update·Dirty Read, 로킹·2PL, 교착상태, MVCC·타임스탬프 | ★★★★★ |
| 3 | [트랜잭션 고립 수준](isolation-level) | Dirty Read·Non-Repeatable Read·Phantom Read, 4단계 Isolation Level | ★★★★★ |
| 4 | [회복 기법](recovery) | REDO·UNDO, 즉시갱신·지연갱신, 체크포인트, 그림자 페이지 | ★★★★☆ |
