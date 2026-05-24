---
title: 최신 데이터 아키텍처
weight: 5
---

빅데이터, 클라우드, AI/LLM 시대를 맞아 데이터베이스 기술은 관계형 모델의 한계를 넘어 다양한 형태로 진화하고 있다. 소셜 미디어·IoT·실시간 스트리밍이 생성하는 비정형 대용량 데이터를 처리하기 위해 NoSQL, 분산 처리 프레임워크, 클라우드 네이티브 DB가 등장하였고, 최근에는 LLM과 연계된 벡터 데이터베이스까지 주목받고 있다.

이 단원에서는 전통적 RDBMS의 ACID를 대체하는 NoSQL의 BASE 특성과 4가지 데이터 모델, 데이터 웨어하우스에서 레이크하우스로의 빅데이터 스토리지 진화, 그리고 클라우드 네이티브 DB와 특수 목적 DB(시계열·벡터)를 다룬다.

## 토픽 목록

| 토픽 | 핵심 내용 | 주요 키워드 |
|---|---|---|
| **NoSQL** | BASE 특성과 Key-Value·Document·Column-Family·Graph 4가지 데이터 모델로 대규모 분산 처리를 실현하는 비관계형 DB | BASE, Eventual Consistency, Redis, MongoDB, Cassandra, Neo4j |
| **빅데이터 스토리지 및 분석 아키텍처** | 데이터 웨어하우스·데이터 레이크·레이크하우스의 진화와 Hadoop·Spark 기반 대용량 분산 처리 프레임워크 | DW, Data Lake, Lakehouse, Hadoop, Spark, Lambda 아키텍처 |
| **클라우드 데이터베이스** | DBaaS 기반 완전 관리형 DB 서비스와 시계열·벡터 DB의 IoT·LLM/RAG 연계 활용 | DBaaS, Aurora Serverless, InfluxDB, Vector DB, RAG, HNSW |
