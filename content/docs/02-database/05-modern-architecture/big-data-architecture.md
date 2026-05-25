---
title: 빅데이터 스토리지 및 분석 아키텍처
description: 데이터 웨어하우스·데이터 레이크·레이크하우스의 진화와 Hadoop·Spark 분산 처리 프레임워크
type: docs
weight: 2
---

## 1. 수집→저장→분석 일괄 처리하는 빅데이터 생태계, 빅데이터 아키텍처의 개요

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    A["기존 DW 한계<br/>비정형 데이터 저장 불가<br/>고비용·수직 확장"] --"분산 저장·처리<br/>스키마 온 리드"--> B["빅데이터 플랫폼<br/>레이크하우스 + Spark<br/>Lambda 아키텍처"] --"배치·실시간 통합<br/>ML·BI 분석"--> C["데이터 드리븐 기업<br/>실시간 인사이트<br/>비용 효율 분석"]
    style A fill:#FFEBEE,stroke:#D32F2F,color:#000
    style B fill:#E3F2FD,stroke:#1976D2,color:#000
    style C fill:#E8F5E9,stroke:#388E3C,color:#000
```

**정의**: 정형·반정형·비정형 대용량 데이터를 수집·저장·처리·분석하는 분산 컴퓨팅 기반의 데이터 플랫폼 아키텍처로, Volume·Velocity·Variety·Veracity 4V 특성의 데이터를 다룬다.
- 데이터 저장소는 스키마 온 라이트(DW)에서 스키마 온 리드(Data Lake)로, 그리고 두 장점을 통합한 레이크하우스로 진화하고 있다.
- 처리 프레임워크는 Hadoop MapReduce 기반 배치에서 Spark 인메모리 처리, 그리고 Flink 실시간 스트림 처리로 발전하였다.
- Lambda 아키텍처와 Kappa 아키텍처가 배치·실시간 데이터를 통합 처리하는 대표적 설계 패턴이다.

**특징**:
- **다중 데이터 형태 처리**: 정형(CSV·DB), 반정형(JSON·XML·로그), 비정형(이미지·동영상·텍스트) 데이터를 단일 플랫폼에서 통합 관리
- **선형 확장 분산 처리**: 수백~수천 노드의 상용 서버로 페타바이트 규모 데이터를 병렬 처리하는 수평 확장 아키텍처
- **비용 효율 저장**: 고가 SAN 대신 저렴한 상용 디스크와 오브젝트 스토리지(S3)를 활용하여 TB당 저장 비용을 1/10 수준으로 절감

---

## 2. 빅데이터 아키텍처의 핵심 구성 체계

### 가. 데이터 저장소 아키텍처 진화

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    DW["데이터 웨어하우스 DW<br/>정형 데이터<br/>스키마 온 라이트<br/>OLAP 최적화"]

    DM["데이터 마트<br/>Data Mart<br/>DW의 부서별 서브셋<br/>도메인 특화"]

    DL["데이터 레이크<br/>Data Lake<br/>모든 원시 데이터<br/>스키마 온 리드"]

    LH["데이터 레이크하우스<br/>Lakehouse<br/>DW 관리성 + 레이크 유연성<br/>Delta Lake·Iceberg"]

    DW --> DM
    DW --> DL
    DL --> LH

    style DW fill:#E3F2FD,stroke:#1976D2,color:#000
    style DM fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style DL fill:#FFF3E0,stroke:#F57C00,color:#000
    style LH fill:#1E3A5F,stroke:#1E3A5F,color:#fff
```

**데이터 웨어하우스(DW) 다차원 모델링**:
- **MOLAP(Multidimensional OLAP)**: 데이터를 하이퍼큐브 형태로 사전 집계 저장, 조회 속도 최고, 저장 공간 비효율
- **ROLAP(Relational OLAP)**: 관계형 DB에 스타·스노우플레이크 스키마로 저장, 유연하지만 집계 속도 느림
- **HOLAP(Hybrid OLAP)**: MOLAP + ROLAP 혼합, 요약 데이터는 큐브·상세 데이터는 관계형으로 저장

| 스토리지 유형 | 데이터 형태 | 스키마 방식 | 강점 | 약점 |
|---|---|---|---|---|
| **데이터 웨어하우스** | 정형 데이터, ETL 전처리 완료 | 스키마 온 라이트 (사전 정의) | 고성능 OLAP 쿼리, 데이터 품질 보장 | 비정형 처리 불가, 스키마 변경 비용 |
| **데이터 마트** | DW 서브셋, 부서별 특화 | 스키마 온 라이트 | 빠른 도메인 분석, 사용자 이해 쉬움 | DW 의존, 중복 데이터 관리 부담 |
| **데이터 레이크** | 정형·반정형·비정형 원시 데이터 | 스키마 온 리드 (사용 시 정의) | 모든 데이터 저장, 저비용 오브젝트 스토리지 | 거버넌스 부재 시 데이터 늪(Data Swamp) |
| **데이터 레이크하우스** | 정형·비정형 통합, ACID 지원 | 메타데이터 관리 + 스키마 진화 | DW 성능 + 레이크 유연성, 통합 거버넌스 | 신기술 도입 복잡도, 성숙도 발전 중 |
| **데이터 메시(Data Mesh)** | 도메인별 분산 관리 | 도메인 오너십 기반 | 조직 자율성, 확장성 | 거버넌스 표준화 도전 |

---

### 나. 대용량 처리 프레임워크

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart TD
    subgraph HADOOP["Hadoop 에코시스템 - 디스크 기반"]
        direction LR
        HDFS["HDFS<br/>분산 파일 시스템<br/>블록 복제"] --> MR["MapReduce<br/>배치 처리<br/>Map + Reduce"]
        YARN["YARN<br/>리소스 관리자"] --> MR
        style HDFS fill:#FFF3E0,stroke:#F57C00,color:#000
        style MR fill:#FFF3E0,stroke:#F57C00,color:#000
        style YARN fill:#FFF3E0,stroke:#F57C00,color:#000
    end

    subgraph SPARK["Apache Spark - 인메모리 기반"]
        direction LR
        SC["Spark Core<br/>RDD·DataFrame<br/>인메모리 엔진"] --> SQL["Spark SQL<br/>구조화 데이터"]
        SC --> SS["Spark Streaming<br/>마이크로 배치"]
        SC --> ML["MLlib<br/>머신러닝"]
        SC --> GX["GraphX<br/>그래프 처리"]
        style SC fill:#E3F2FD,stroke:#1976D2,color:#000
        style SQL fill:#E3F2FD,stroke:#1976D2,color:#000
        style SS fill:#E3F2FD,stroke:#1976D2,color:#000
        style ML fill:#E3F2FD,stroke:#1976D2,color:#000
        style GX fill:#E3F2FD,stroke:#1976D2,color:#000
    end

    subgraph LAMBDA["Lambda 아키텍처"]
        direction LR
        RAW["원시 데이터"] --> BATCH["배치 레이어<br/>Hadoop·Spark<br/>전체 재처리"]
        RAW --> SPEED["스피드 레이어<br/>Flink·Kafka Streams<br/>실시간 처리"]
        BATCH --> SERVING["서빙 레이어<br/>Druid·Cassandra<br/>쿼리 응답"]
        SPEED --> SERVING
        style RAW fill:#FFEBEE,stroke:#D32F2F,color:#000
        style BATCH fill:#FFF3E0,stroke:#F57C00,color:#000
        style SPEED fill:#F3E5F5,stroke:#7B1FA2,color:#000
        style SERVING fill:#E8F5E9,stroke:#388E3C,color:#000
    end

    HADOOP --> SPARK
    SPARK --> LAMBDA
```

| 프레임워크 | 처리 방식 | 강점 | 약점 | 적합 케이스 |
|---|---|---|---|---|
| **Hadoop MapReduce** | 디스크 기반 배치 처리, Map→Shuffle→Reduce | 안정성 검증, 페타바이트 처리, 오픈소스 생태계 | 디스크 I/O 병목으로 느린 처리, 반복 처리 비효율 | 로그 분석, 대용량 ETL, 야간 배치 처리 |
| **Apache Spark** | 인메모리 RDD·DataFrame, DAG 최적화 실행 | MapReduce 대비 배치 10배·인메모리 100배 빠름 | 메모리 비용, OOM 위험, 소용량 데이터 비효율 | 머신러닝, 대화형 분석, 스트리밍, SQL 쿼리 |
| **Apache Flink** | 진정한 이벤트 기반 스트림 처리, Stateful 연산 | 실시간 처리 최저 지연, 정확한 Once 처리 보장 | 학습 곡선, 배치 처리 최적화 미흡 | 실시간 사기 탐지, 이벤트 드리븐 처리 |
| **Kafka Streams** | Kafka 토픽 기반 스트림 처리, 경량 라이브러리 | 별도 클러스터 불필요, Kafka 생태계 통합 | 처리 복잡도 한계, 배치 미지원 | 실시간 데이터 변환, 이벤트 집계 |
| **Delta Lake / Iceberg** | 레이크하우스 테이블 포맷, ACID 트랜잭션 지원 | 스키마 진화, 타임 트래블, 통계 기반 최적화 | 추가 메타데이터 관리, 소파일 문제 | Lakehouse 기반 ACID 보장 분석 워크로드 |

---

## 3. 빅데이터 아키텍처 도입의 기대효과 및 활용 방안

| 구분 | 주요 기대효과 | 활용 및 실무 적용 방안 |
|---|---|---|
| **비용 절감** | 고가 DW 대신 오브젝트 스토리지(S3) + Spark 조합으로 TB당 비용 1/10 수준 달성 | AWS S3 기반 레이크하우스(Delta Lake)로 기존 Teradata·Oracle DW 마이그레이션 |
| **처리 속도** | Spark 인메모리 처리로 일 단위 배치를 시간·분 단위로 단축하여 의사결정 지연 최소화 | 야간 배치 ETL 파이프라인을 Spark로 전환하여 D+1 리포트를 실시간 대시보드로 전환 |
| **데이터 통합** | 정형·비정형 데이터를 단일 레이크에 통합하여 데이터 사일로 해소 및 통합 분석 가능 | 고객 구매 이력(DB) + 소셜 미디어 텍스트 + 앱 로그를 통합하여 360도 고객 뷰 구축 |
| **AI/ML 연계** | MLlib·TensorFlow on Spark 등을 활용하여 대용량 데이터 기반 머신러닝 파이프라인 구현 | 피처 엔지니어링부터 모델 학습·서빙까지 Spark 기반 ML 파이프라인 자동화 구축 |
