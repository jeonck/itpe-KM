# Hugo Hextra 테마 설정 및 GitHub Pages 배포 가이드

이 문서는 `itpe-KM` 지식베이스 구축 과정에서 적용된 설정 사항과 배포 관련 문제 해결 내용을 기록합니다.

## 1. 프로젝트 개요
- **프레임워크**: Hugo (Extended version)
- **테마**: [Hextra](https://github.com/imfing/hextra)
- **목적**: 정보시스템 기술사 지식베이스 구축
- **주요 기능**: Mermaid 다이어그램, LaTeX 수식, FlexSearch 검색 지원

## 2. 테마 설정 (`hugo.yaml`)
Hextra 테마의 기능을 활성화하기 위해 다음과 같이 설정했습니다.

```yaml
theme: hextra

params:
  search:
    enable: true
    type: flexsearch
  latex:
    enable: true
  mermaid:
    enable: true
```

- **Mermaid/LaTeX**: `goldmark` 렌더러에서 `unsafe: true` 설정이 필요할 수 있으며, Hextra는 자체 파라미터를 통해 이를 간편하게 지원합니다.
- **검색**: FlexSearch를 사용하여 별도의 서버 없이 클라이언트 사이드에서 빠른 검색을 지원합니다.

## 3. GitHub Pages 배포 해결 과정

### 문제 발생
초기 GitHub Actions 워크플로우 설정 시 `This run likely failed because of a workflow file issue` 오류와 함께 배포가 실패하는 현상이 발생했습니다.

### 해결 방법
검증된 워크플로우 템플릿을 적용하여 해결했습니다. 주요 포인트는 다음과 같습니다.

1. **Permissions 설정**: `contents: read`, `pages: write`, `id-token: write` 권한 명시.
2. **Setup Pages**: `actions/configure-pages@v5`를 사용하여 `base_url`을 동적으로 획득.
3. **Build 명령어**: `--baseURL` 옵션을 통해 GitHub Pages의 서브 디렉토리 경로가 올바르게 인식되도록 처리.
4. **Deploy 단계**: `actions/deploy-pages@v4`를 사용하여 빌드된 `public` 폴더를 배포.

### 최종 워크플로우 (`.github/workflows/deploy.yaml`)
```yaml
name: Deploy Hugo to GitHub Pages

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true

      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5

      - name: Build
        run: hugo --minify --baseURL "${{ steps.pages.outputs.base_url }}/"

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

## 4. 로컬 작업 가이드
변경 사항을 로컬에서 확인하려면 다음 명령어를 사용하세요.

```bash
# 테마 서브모듈 포함 클론
git clone --recursive https://github.com/jeonck/itpe-KM.git

# 로컬 서버 실행
hugo server -D
```
