---
title: 데이터베이스 성능 최적화
weight: 3
---

데이터베이스 성능 최적화는 쿼리 응답 시간 단축과 시스템 처리량 향상을 위해 인덱스 설계, 옵티마이저 활용, 조인 알고리즘 선택, 데이터 분산 전략을 체계적으로 적용하는 핵심 기술 영역이다. 기술사 시험에서 B+Tree 인덱스 구조, CBO 옵티마이저 비용 계산, 조인 알고리즘 비교, 파티셔닝 유형은 단골 출제 항목이며, 대용량 서비스 아키텍처 설계의 기초가 된다.

| 순서 | 토픽 | 핵심 키워드 | 중요도 |
|:---:|:---|:---|:---:|
| 1 | [인덱스](index-structure) | B+Tree·비트맵·해시, 클러스터드·넌클러스터드, Index Range Scan·Unique Scan | ★★★★★ |
| 2 | [옵티마이저 및 실행계획](optimizer) | RBO·CBO, 비용 산정, 힌트(Hint), EXPLAIN PLAN, 통계 정보 | ★★★★★ |
| 3 | [조인 메커니즘](join-mechanism) | Nested Loop·Sort Merge·Hash Join, 드라이빙 테이블, 조인 순서 최적화 | ★★★★☆ |
| 4 | [파티셔닝 및 샤딩](partitioning-sharding) | Range·List·Hash·Composite Partitioning, Shard Key, 재샤딩 | ★★★★☆ |
