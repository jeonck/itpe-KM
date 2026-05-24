# LaTeX 렌더링 문제 해결 가이드

이 문서는 `itpe-KM` 지식베이스 구축 중 발생한 LaTeX 수식 렌더링 오류의 원인과 해결 과정을 기록합니다.

## 1. 문제 현상
- `content/docs/test.md` 파일에 작성된 LaTeX 문법(`$ ... $`, `$$ ... $$`)이 수식으로 변환되지 않고 일반 텍스트로 노출됨.
- Hextra 테마 내 KaTeX 스크립트가 활성화되지 않음.

## 2. 원인 분석
1. **Hugo Goldmark 설정**: Hugo의 기본 Markdown 렌더러인 Goldmark는 보안 및 표준 준수를 위해 수식 구분자(`$`)를 기본적으로 무시합니다. 이를 명시적으로 허용하는 `passthrough` 설정이 필요했습니다.
2. **테마 파라미터 불일치**: 초기 설정 시 사용한 `params.latex` 대신 Hextra 테마가 내부적으로 사용하는 `params.math` 구조를 적용해야 했습니다.

## 3. 해결 방법 (`hugo.yaml` 수정)

### 3.1. Goldmark Passthrough 설정
Markdown 파일 내의 수식 구분자를 해석할 수 있도록 `markup` 섹션을 다음과 같이 수정했습니다.

```yaml
markup:
  goldmark:
    renderer:
      unsafe: true # HTML 태그 허용 (필요한 경우)
    extensions:
      passthrough:
        delimiters:
          block: [['\[', '\]'], ['$$', '$$']] # 블록 수식 구분자
          inline: [['\(', '\)'], ['$', '$']]  # 인라인 수식 구분자
        enable: true
```

### 3.2. Math 파라미터 활성화
테마에서 KaTeX 엔진을 로드할 수 있도록 파라미터를 업데이트했습니다.

```yaml
params:
  math:
    enable: true
```

## 4. LaTeX 작성 규칙 (지원되는 구분자)
현재 설정에 따라 다음 네 가지 방식으로 수식을 작성할 수 있습니다.

- **인라인 수식**: `$E = mc^2$` 또는 `\( E = mc^2 \)`
- **블록 수식**:
  ```latex
  $$
  x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}
  $$
  ```
  또는
  ```latex
  \[
  \int_{a}^{b} f(x) dx
  \]
  ```

## 5. 확인 방법
수정 사항 적용 후 GitHub Pages에 배포된 `/docs/test/` 페이지에서 수식이 미려하게 렌더링되는지 확인합니다. 렌더링이 안 될 경우 브라우저 개발자 도구(F12)의 Console 탭에서 KaTeX 관련 로딩 에러가 있는지 체크합니다.
