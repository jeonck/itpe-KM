---
title: 공급망 보안 및 SBOM
description: 오픈소스 취약점 관리와 SBOM(소프트웨어 자재 명세서) 표준(SPDX·CycloneDX) 필요성과 구축 방안
type: docs
weight: 4
---

## 1. 소프트웨어 공급망 가시성과 SBOM으로 취약점 추적, 공급망 보안 및 SBOM의 개요

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    A["공급망 공격 급증<br/>Log4Shell·SolarWinds"] --"SBOM 생성·관리<br/>SCA 자동 분석"--> B["소프트웨어 구성 가시성<br/>취약 컴포넌트 즉시 파악"] --"신속 패치·<br/>라이선스 준수"--> C["공급망 신뢰성 확보<br/>규정 준수 자동화"]
    style A fill:#FFEBEE,stroke:#D32F2F,color:#000
    style B fill:#E3F2FD,stroke:#1976D2,color:#000
    style C fill:#E8F5E9,stroke:#388E3C,color:#000
```

**정의**: 소프트웨어를 구성하는 모든 오픈소스·써드파티 컴포넌트를 SBOM으로 목록화하여 공급망 공격과 오픈소스 취약점을 추적·관리하는 보안 체계.
- SolarWinds·Log4Shell·XZ Utils 사례로 공급망 공격의 파급력과 탐지 어려움이 입증됨
- SBOM은 컴포넌트명·버전·라이선스·의존성을 기계 판독 가능한 형식으로 기록
- 미국 행정명령(EO 14028) 및 CISA 가이드라인으로 연방 조달 시 SBOM 제출이 의무화 추진 중

**특징**:
- **공급망 가시성**: 소프트웨어에 포함된 모든 컴포넌트와 의존성을 투명하게 추적하여 취약점 영향 범위 즉시 파악
- **신속 대응**: 신규 CVE 발표 시 영향받는 컴포넌트를 SBOM에서 즉시 조회하여 패치·교체 우선순위 결정
- **라이선스 컴플라이언스**: GPL·MIT·Apache 등 라이선스 의무 사항을 SBOM 기반으로 자동 검증, 법적 리스크 감소

---

## 2. 공급망 보안 및 SBOM의 핵심 구성 체계

### 가. 공급망 보안 위협 및 오픈소스 취약점

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    SUP["소프트웨어 공급자<br/>오픈소스·써드파티"] --"빌드 파이프라인<br/>CI/CD 침해"--> BUILD["빌드·배포 단계<br/>악성코드 삽입"]
    BUILD --"패키지 배포<br/>타이포스쿼팅"--> DIST["배포 채널<br/>npm·PyPI·Maven"]
    DIST --"취약 컴포넌트<br/>포함"--> ORG["피해 조직<br/>운영 시스템 침해"]
    style SUP fill:#FFEBEE,stroke:#D32F2F,color:#000
    style BUILD fill:#FFF3E0,stroke:#F57C00,color:#000
    style DIST fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style ORG fill:#FFEBEE,stroke:#D32F2F,color:#000
```

| 공격명 | 침투 경로 | 피해 | 대응 |
|---|---|---|---|
| **SolarWinds** | 빌드 서버 침해 → 업데이트 파일에 백도어 삽입 | 18,000개 이상 기관 침해 | 빌드 환경 무결성 검증, 코드 서명 |
| **Log4Shell** | Log4j 취약점(CVE-2021-44228) JNDI 인젝션 | 수억 대 서버 원격 코드 실행 | SCA로 Log4j 사용 즉시 탐지·패치 |
| **타이포스쿼팅** | 유사 패키지명으로 악성 패키지 공개 저장소 등록 | 개발자 설치 시 악성코드 실행 | 패키지 해시 검증, 공급망 정책 |
| **XZ Utils** | 오픈소스 기여자로 위장, 백도어 장기 삽입 | SSH 데몬 인증 우회 가능성 | 기여자 검증, 코드 리뷰 강화 |

---

### 나. SBOM (Software Bill of Materials)

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    SRC["소스코드<br/>오픈소스 포함"] --"SCA 도구<br/>분석"--> SCA["SCA 분석<br/>Syft·Trivy·FOSSA"]
    SCA --"자동 생성"--> SBOM["SBOM<br/>SPDX·CycloneDX"]
    SBOM --"CVE 매핑"--> VDB["취약점 DB<br/>NVD·OSV"]
    VDB --"영향 컴포넌트<br/>즉시 조회"--> RESP["취약점 대응<br/>패치·교체·격리"]
    style SRC fill:#E3F2FD,stroke:#1976D2,color:#000
    style SCA fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style SBOM fill:#1E3A5F,stroke:#1E3A5F,color:#fff
    style VDB fill:#FFF3E0,stroke:#F57C00,color:#000
    style RESP fill:#E8F5E9,stroke:#388E3C,color:#000
```

| 구분 | SPDX | CycloneDX |
|---|---|---|
| **개발 기관** | Linux Foundation | OWASP |
| **형식** | RDF·TV·JSON·YAML | XML·JSON·Protocol Buffers |
| **초점** | 라이선스 컴플라이언스·ISO/IEC 5962 표준 | 보안 취약점·VEX 연동 중심 |
| **주요 활용** | 오픈소스 라이선스 감사, 공급망 투명성 | CI/CD 보안 게이트, 취약점 추적 |

---

## 3. 공급망 보안 및 SBOM 도입의 기대효과 및 활용 방안

| 구분 | 주요 기대효과 | 활용 및 실무 적용 방안 |
|---|---|---|
| **가시성** | 소프트웨어 구성 요소 전수 파악, 숨겨진 취약 컴포넌트 투명화 | CI/CD 파이프라인에 SCA 도구 통합, SBOM 자동 생성 |
| **신속 대응** | 신규 CVE 발표 즉시 영향 컴포넌트 조회, 패치 우선순위 자동 결정 | NVD·OSV 취약점 DB 연동, 영향 SBOM 항목 자동 알림 |
| **라이선스 관리** | GPL·LGPL 등 카피레프트 의무 사항 자동 탐지, 법적 리스크 사전 제거 | FOSSA·Black Duck 기반 라이선스 정책 자동 적용 |
| **규정 준수** | 미국 EO 14028·CISA 가이드라인 대응, 공공·금융 조달 요건 충족 | CycloneDX SBOM 납품 첨부, SPDX ISO 표준 기반 감사 증적 |
