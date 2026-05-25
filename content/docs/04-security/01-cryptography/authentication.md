---
title: 사용자 인증 기술
description: 지식·소유·존재·행위 4대 인증 요소, MFA·FIDO·Passkey, SSO·OAuth 2.0·OIDC·SAML 통합 인증
type: docs
weight: 5
---

## 1. 다중 요소·생체·통합 인증으로 신원 검증 강화, 사용자 인증 기술의 개요

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    A["단일 패스워드<br/>피싱·크리덴셜 탈취 취약"] --"MFA·FIDO2·<br/>통합 인증 도입"--> B["다중 요소 인증<br/>SSO·OAuth·OIDC·SAML"] --"신원 강화·<br/>편의성 확보"--> C["Zero Trust 기반<br/>안전한 통합 접근"]
    style A fill:#FFEBEE,stroke:#D32F2F,color:#000
    style B fill:#E3F2FD,stroke:#1976D2,color:#000
    style C fill:#E8F5E9,stroke:#388E3C,color:#000
```

**정의**: 지식·소유·존재·행위 4대 요소를 단독 또는 조합하여 사용자 신원을 검증하는 인증 기술 체계.
- MFA는 2개 이상의 인증 요소를 결합하여 단일 요소 탈취 시에도 보안을 유지
- FIDO2/WebAuthn·Passkey는 비밀번호 없는 인증으로 피싱 공격을 원천 차단
- OAuth 2.0·OIDC·SAML은 다중 서비스 환경에서 신원 연합과 권한 위임을 표준화

**특징**:
- **다중 요소 결합**: 지식·소유·생체 요소 조합으로 단일 요소 침해 시에도 계정 보호
- **비밀번호 탈피**: FIDO2·Passkey로 기기 내 개인키 기반 인증, 피싱·리플레이 공격 불가
- **연합 인증**: SSO·OIDC·SAML로 1회 인증 후 다수 서비스 접근, 사용자 경험 향상

---

## 2. 사용자 인증 기술의 핵심 구성 체계

### 가. 인증 4대 요소, MFA, FIDO2·Passkey

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    SRV["서버<br/>Challenge 전송"]
    DEV["인증 기기<br/>개인키로 서명"]
    VRF["서버 검증<br/>공개키로 서명 확인"]
    OK["인증 성공<br/>세션 발급"]

    SRV --> DEV
    DEV --> VRF
    VRF --> OK

    style SRV fill:#E3F2FD,stroke:#1976D2,color:#000
    style DEV fill:#1E3A5F,stroke:#1E3A5F,color:#fff
    style VRF fill:#FFF3E0,stroke:#F57C00,color:#000
    style OK fill:#E8F5E9,stroke:#388E3C,color:#000
```

| 인증 요소 | 예시 | 장점 | 단점 |
|---|---|---|---|
| **지식 (Know)** | 패스워드, PIN, 보안 질문 | 구현 단순, 비용 최소 | 피싱·브루트포스 취약 |
| **소유 (Have)** | OTP, 스마트카드, 보안 토큰 | 물리적 탈취 필요 → 보안 강화 | 분실·도난 시 접근 불가 |
| **존재 (Are)** | 지문, 홍채, 얼굴, 정맥 인식 | 복제 어려움, 편의성 우수 | 생체 정보 변경 불가, 프라이버시 |
| **행위 (Do)** | 서명 패턴, 걸음걸이, 타이핑 리듬 | 지속적 인증 가능 | 환경 변화에 민감, 정확도 편차 |

---

### 나. 통합 인증 (SSO·OAuth 2.0·OIDC·SAML)

```mermaid
%%{init: { 'theme': 'base', 'themeVariables': { 'edgeLabelBackground': '#fff' }}}%%
flowchart LR
    USER["사용자<br/>Authorization 요청"]
    AS["인가 서버<br/>Authorization Code 발급"]
    CB["클라이언트<br/>Code → Token 교환"]
    RS["리소스 서버<br/>Access Token 검증"]
    DATA["보호 자원<br/>응답 반환"]

    USER --> AS
    AS --> CB
    CB --> RS
    RS --> DATA

    style USER fill:#FFF3E0,stroke:#F57C00,color:#000
    style AS fill:#1E3A5F,stroke:#1E3A5F,color:#fff
    style CB fill:#E3F2FD,stroke:#1976D2,color:#000
    style RS fill:#F3E5F5,stroke:#7B1FA2,color:#000
    style DATA fill:#E8F5E9,stroke:#388E3C,color:#000
```

| 프로토콜 | 목적 | 형식 | 사용 환경 | 핵심 특징 |
|---|---|---|---|---|
| **SSO** | 1회 인증·다중 서비스 | 세션·토큰 기반 | 기업 내부 포털 | 중앙 인증, UX 향상 |
| **OAuth 2.0** | 권한 위임·리소스 접근 | Access Token (Bearer) | 소셜 로그인, API | 신원 인증 미포함, 권한 위임 전용 |
| **OIDC** | 신원 인증 + 권한 위임 | JWT (ID Token) | 웹·모바일 서비스 | OAuth 2.0 상위 레이어, 표준 클레임 |
| **SAML 2.0** | 기업 연합 인증 (SSO) | XML Assertion | 기업·교육·공공 | IdP·SP 연합, 레거시 호환성 우수 |

---

## 3. 사용자 인증 기술 도입의 기대효과 및 활용 방안

| 구분 | 주요 기대효과 | 활용 및 실무 적용 방안 |
|---|---|---|
| **보안성** | MFA·FIDO2로 크리덴셜 기반 공격 대폭 감소 | 관리자 계정 MFA 의무화, Passkey 기반 비밀번호리스 전환 |
| **편의성** | SSO·OIDC로 1회 로그인 후 다중 서비스 원활 접근 | Azure AD·Okta 연동 SSO 구축, 직원 경험 향상 |
| **표준화** | OAuth 2.0·OIDC·SAML로 이기종 시스템 인증 통합 | 사내 SaaS·온프레미스 통합 IdP 운영, API 보안 표준화 |
| **규제 준수** | 전자금융감독규정·개인정보보호법 인증 요건 충족 | 금융 서비스 2FA 의무화, 공공기관 FIDO 인증 연계 적용 |
