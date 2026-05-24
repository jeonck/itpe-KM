---
title: 데이터 모델링
description: 개념-논리-물리 3단계 변환으로 현실 세계를 데이터베이스 구조로 추상화하는 설계 기법
type: docs
weight: 2
---

## 1. 현실 세계를 3단계 추상화로 DB 구조로 변환하는 체계적 설계 기법, 데이터 모델링의 개요

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    A["비정형 현실 세계<br/>업무 요구사항<br/>복잡한 관계 구조"] --"추상화·개념화<br/>3단계 변환 프로세스"--> B["데이터 모델링<br/>개념→논리→물리<br/>단계적 정제"] --"구현 가능한 설계<br/>구조·운영·제약 정의"--> C["최적화된 DB 스키마<br/>유지보수 용이성<br/>성능·확장성 확보"]
    style A fill:#FFEBEE,stroke:#D32F2F,color:#000
    style B fill:#E3F2FD,stroke:#1976D2,color:#000
    style C fill:#E8F5E9,stroke:#388E3C,color:#000
```

**정의**: 현실 세계의 업무 개념과 관계를 개념적·논리적·물리적 3단계로 단계적으로 추상화하여 데이터베이스 구조를 설계하는 체계적 기법.
- 개념적 모델링에서 E-R 다이어그램으로 비즈니스 엔티티·관계를 표현하고, 논리적 모델링에서 릴레이션 스키마로 변환
- 물리적 모델링에서 인덱스·파티션·저장 구조를 결정하여 DBMS 최적화된 최종 스키마 산출
- 요구사항 변경에 유연하게 대응하고 개발팀·도메인 전문가 간 의사소통 도구로 활용

**특징**:
- **단계적 정제**: 추상도가 높은 개념 모델부터 구현 수준의 물리 모델까지 점진적으로 세부화하여 오류를 조기 발견
- **독립적 표현**: 특정 DBMS에 종속되지 않는 논리 모델을 중심으로 물리 모델을 다양한 플랫폼에 매핑 가능
- **커뮤니케이션 수단**: E-R 다이어그램은 비전산 부서 담당자도 이해할 수 있는 공용 언어로 기능하며 요구사항 검증에 활용

---

## 2. 데이터 모델링의 핵심 구성 체계

### 가. 데이터 모델링 3단계 절차와 E-R 다이어그램 구성 요소

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    S1["개념적 모델링<br/>E-R 다이어그램<br/>엔티티·관계·속성 식별"] --> S2["논리적 모델링<br/>릴레이션 스키마<br/>정규화·키 정의"] --> S3["물리적 모델링<br/>테이블·인덱스<br/>파티션·저장 구조"]
    style S1 fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style S2 fill:#E3F2FD,stroke:#1976D2,color:#000
    style S3 fill:#E8F5E9,stroke:#388E3C,color:#000
```

**E-R 다이어그램 핵심 구성 요소**

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart TD
    ROOT["E-R 다이어그램<br/>구성 요소"] --> E["엔티티<br/>Entity"]
    ROOT --> R["관계<br/>Relationship"]
    ROOT --> A["속성<br/>Attribute"]
    E --> E1["강한 엔티티<br/>독립 존재 가능"]
    E --> E2["약한 엔티티<br/>부모 엔티티 의존"]
    R --> R1["1:1 관계<br/>단방향 대응"]
    R --> R2["1:N 관계<br/>일대다 대응"]
    R --> R3["M:N 관계<br/>다대다 연결테이블"]
    A --> A1["단순 속성<br/>더 이상 분해 불가"]
    A --> A2["복합 속성<br/>여러 단순 속성 합성"]
    A --> A3["다중값 속성<br/>복수 값 가능"]
    style ROOT fill:#1E3A5F,stroke:#1E3A5F,color:#fff
    style E fill:#E3F2FD,stroke:#1976D2,color:#000
    style R fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style A fill:#FFF3E0,stroke:#F57C00,color:#000
    style E1 fill:#E3F2FD,stroke:#1976D2,color:#000
    style E2 fill:#E3F2FD,stroke:#1976D2,color:#000
    style R1 fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style R2 fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style R3 fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style A1 fill:#FFF3E0,stroke:#F57C00,color:#000
    style A2 fill:#FFF3E0,stroke:#F57C00,color:#000
    style A3 fill:#FFF3E0,stroke:#F57C00,color:#000
```

| 모델링 단계 | 산출물 | 주요 활동 | 참여자 |
|:---:|:---|:---|:---|
| **개념적 모델링** | E-R 다이어그램 | 엔티티 식별, 관계 정의, 핵심 속성 도출, 카디널리티 결정 | 업무 담당자, DA |
| **논리적 모델링** | 릴레이션 스키마 | E-R → 테이블 변환, 정규화, PK·FK 정의, 도메인 설정 | DBA, DA |
| **물리적 모델링** | DDL 스크립트 | 인덱스 설계, 파티션 전략, 저장 파라미터, 클러스터링 결정 | DBA, 시스템 엔지니어 |

**E-R 다이어그램 → 릴레이션 스키마 변환 규칙**

| 변환 대상 | 변환 규칙 | 결과 |
|:---:|:---|:---|
| **강한 엔티티** | 엔티티 → 독립 테이블, 기본키 속성 → PK | 단독 테이블 생성 |
| **약한 엔티티** | 부분 키 + 부모 PK → 복합 PK | 부모 FK 포함 테이블 |
| **1:1 관계** | 참여도 낮은 쪽 테이블에 FK 추가 또는 테이블 병합 | FK 추가 또는 단일 테이블 |
| **1:N 관계** | N 쪽 테이블에 1 쪽의 PK를 FK로 추가 | N 쪽에 FK 컬럼 |
| **M:N 관계** | 별도 연결 테이블 생성, 양쪽 PK → 복합 PK | 교차 테이블 생성 |
| **다중값 속성** | 별도 테이블로 분리, 원본 엔티티 PK → FK | 1:N 구조의 별도 테이블 |

---

### 나. 데이터 모델의 표시 요소와 물리 모델링 설계 전략

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart TD
    ROOT["데이터 모델<br/>3대 표시 요소"] --> S["구조<br/>Structure"]
    ROOT --> O["연산<br/>Operation"]
    ROOT --> C["제약조건<br/>Constraint"]
    S --> S1["엔티티·속성·관계<br/>스키마 정적 구조"]
    O --> O1["삽입·삭제·수정·검색<br/>데이터 조작 연산"]
    C --> C1["도메인·무결성<br/>비즈니스 규칙 적용"]
    style ROOT fill:#1E3A5F,stroke:#1E3A5F,color:#fff
    style S fill:#E3F2FD,stroke:#1976D2,color:#000
    style O fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style C fill:#FFF3E0,stroke:#F57C00,color:#000
    style S1 fill:#E3F2FD,stroke:#1976D2,color:#000
    style O1 fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style C1 fill:#FFF3E0,stroke:#F57C00,color:#000
```

**물리 모델링 핵심 설계 요소**

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    P1["인덱스 설계<br/>B-Tree·Bitmap<br/>복합·함수 인덱스"] --> P2["파티셔닝<br/>Range·Hash·List<br/>복합 파티션"] --> P3["클러스터링<br/>IOT·클러스터 테이블<br/>데이터 배치 최적화"] --> P4["저장 구조<br/>테이블스페이스<br/>블록 크기·Row Migration"]
    style P1 fill:#E3F2FD,stroke:#1976D2,color:#000
    style P2 fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style P3 fill:#FFF3E0,stroke:#F57C00,color:#000
    style P4 fill:#E8F5E9,stroke:#388E3C,color:#000
```

| 물리 설계 요소 | 설계 기준 | 성능 효과 | 주의사항 |
|:---:|:---|:---|:---|
| **B-Tree 인덱스** | 카디널리티 높은 컬럼, 범위 검색 조건 | 포인트 검색·범위 검색 10~100배 향상 | DML 성능 저하, 과도한 인덱스 지양 |
| **Bitmap 인덱스** | 카디널리티 낮은 컬럼(성별·상태코드), OLAP | AND/OR 연산 병렬 처리, 집계 쿼리 최적화 | OLTP 환경 부적합, Lock 범위 큼 |
| **Range 파티션** | 날짜·순번 기반 대용량 이력 테이블 | 파티션 프루닝으로 스캔 범위 대폭 축소 | 파티션 키 선정이 성능 결정 |
| **Hash 파티션** | 균등 분산 필요 시, 조인 성능 최적화 | 데이터 편중 없는 균일 분산 보장 | 범위 검색 불가, 파티션 수 2의 배수 권장 |
| **클러스터 인덱스** | 자주 함께 조회되는 테이블 그룹 | 클러스터 키 기반 조인 I/O 최소화 | 클러스터 키 변경 시 데이터 재배치 |

---

## 3. 데이터 모델링 적용의 기대효과 및 활용 방안

| 구분 | 주요 기대효과 | 활용 및 실무 적용 방안 |
|:---:|:---|:---|
| **설계 품질** | 요구사항 오류를 초기 단계에서 발견하여 후속 단계 재작업 비용을 60~80% 절감 | 개념 모델 리뷰 체크리스트 도입, 도메인 전문가와의 E-R 다이어그램 공동 검토 워크숍 운영 |
| **커뮤니케이션** | E-R 다이어그램을 공용 언어로 업무 부서·개발·DBA 간 요구사항 오해 최소화 | 논리 모델 기반 API 계약(Contract) 정의, 마이크로서비스 경계 설계에 도메인 모델 활용 |
| **시스템 유연성** | DBMS 독립적인 논리 모델 유지로 클라우드 전환·멀티 DB 아키텍처 변환 용이 | 논리 모델 리포지터리 관리, DDL 자동 생성 도구(ERwin·DataGrip) 활용으로 물리 모델 자동화 |
| **성능 최적화** | 물리 모델 단계의 인덱스·파티션 사전 설계로 오픈 후 성능 장애 선제 예방 | 쿼리 패턴 분석 기반 인덱스 설계, 대용량 이력 테이블 파티션 전략 수립으로 조회 응답 시간 단축 |
