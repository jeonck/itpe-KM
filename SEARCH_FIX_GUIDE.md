# 검색 기능 활성화 및 노출 문제 해결 가이드

이 문서는 `itpe-KM` 지식베이스 구축 중 검색 기능이 정상적으로 표시되지 않거나 특정 페이지에서 누락되었던 문제를 해결한 과정을 기록합니다.

## 1. 문제 현상
1.  **내비게이션 바 검색창 누락**: `hugo.yaml`에서 `params.search.enable: true` 설정을 했음에도 불구하고 상단 메뉴에 검색창이 나타나지 않음.
2.  **Docs 페이지 사이드바 검색창 누락**: 문서(Docs) 페이지로 이동 시 왼쪽 사이드바에서 검색창이 보이지 않아 접근성이 떨어짐.

## 2. 원인 분석
1.  **Hextra 테마의 메뉴 기반 검색 설계**: Hextra 테마는 단순히 파라미터만 켜는 것이 아니라, `menu.main` 설정에 `type: search` 속성을 가진 아이템이 있어야 내비게이션 바에 검색창을 렌더링합니다.
2.  **사이드바 기본 반응형 설정**: 테마 기본 레이아웃에서 사이드바 검색창은 모바일 화면(`hx:md:hidden`)에서만 보이도록 설정되어 있어, 데스크톱 환경의 Docs 페이지에서는 노출되지 않았습니다.

## 3. 해결 방법

### 3.1. 내비게이션 바 검색창 활성화 (`hugo.yaml`)
메인 메뉴 설정에 검색 타입을 명시적으로 추가했습니다.

```yaml
menu:
  main:
    - name: Docs
      url: /docs/
      weight: 10
    - name: Search
      weight: 20
      params:
        type: search # 이 설정이 검색창을 활성화함
```

### 3.2. Docs 사이드바 검색창 상시 노출 (`layouts/partials/sidebar.html`)
테마의 기본 사이드바 레이아웃을 프로젝트 루트로 복사하여 오버라이드했습니다. `hx:md:hidden` 클래스를 제거하여 모든 화면 크기에서 검색창이 보이도록 수정했습니다.

**수정 전:**
```html
<div class="hx:px-4 hx:pt-4 hx:md:hidden">
  {{ partial "search.html" (dict "location" "sidebar") }}
</div>
```

**수정 후:**
```html
<div class="hx:px-4 hx:pt-4">
  {{ partial "search.html" (dict "location" "sidebar") }}
</div>
```

## 4. 최종 확인
- **상단 내비게이션 바**: 우측에 검색창과 단축키(CTRL K) 안내가 표시됨.
- **문서 사이드바**: 왼쪽 과목 메뉴 상단에 검색창이 상시 노출됨.
- **기능**: FlexSearch 엔진을 통해 문서 제목 및 내용에 대한 실시간 검색이 가능함.

## 5. 유지보수 참고
검색 엔진의 성능이나 인덱싱 범위를 조정하려면 `hugo.yaml`의 `params.search.flexsearch` 섹션을 수정하십시오.
