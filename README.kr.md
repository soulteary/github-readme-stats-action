# Readme Stats Action

[![GitHub](https://img.shields.io/badge/GitHub-soulteary%2Fgithub--readme--stats--action-blue)](https://github.com/soulteary/github-readme-stats-action)

![Readme Stats Action](.github/assets/banner.jpg)

## Languages / 语言 / Sprachen / Lingue / 언어 / 言語

- [English](README.md)
- [简体中文](README.zh.md)
- [Deutsch](README.de.md)
- [Italiano](README.it.md)
- [한국어](README.kr.md)
- [日本語](README.ja.md)

GitHub Actions 워크플로우에서 [GitHub Readme Stats](https://github.com/soulteary/github-readme-stats) 카드를 생성하고, 프로필 저장소에 커밋한 다음, 거기서 직접 임베드합니다.

이 Action은 `github-readme-stats` 서비스의 Go 구현을 사용하며, GitHub Releases에서 사전 컴파일된 바이너리를 다운로드하고 CLI를 통해 호출하여 통계 카드를 생성합니다.

## 빠른 시작

```yaml
name: Update README cards

on:
  schedule:
    - cron: "0 0 * * *" # 매일 자정에 한 번 실행
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    permissions:
      contents: write

    steps:
      - uses: actions/checkout@v4

      - name: Generate stats card
        uses: soulteary/github-readme-stats-action@v1.0.0
        with:
          card: stats
          options: 'username=${{ github.repository_owner }}&show_icons=true'
          path: profile/stats.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Generate top languages card
        uses: soulteary/github-readme-stats-action@v1.0.0
        with:
          card: top-langs
          options: 'username=${{ github.repository_owner }}&layout=compact&langs_count=6'
          path: profile/top-langs.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Generate pin card
        uses: soulteary/github-readme-stats-action@v1.0.0
        with:
          card: pin
          options: 'username=soulteary&repo=github-readme-stats'
          path: profile/pin-github-readme-stats.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Commit cards
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add profile/*.svg
          git commit -m "Update README cards" || exit 0
          git push
```

그런 다음 프로필 README에서 임베드:

```md
![Stats](./profile/stats.svg)
![Top Languages](./profile/top-langs.svg)
![Pinned](./profile/pin-github-readme-stats.svg)
```

## 배포 옵션

이 action은 권장되는 배포 옵션 중 하나입니다. Vercel 또는 다른 플랫폼에서도 배포할 수 있습니다. [GitHub Readme Stats README](https://github.com/soulteary/github-readme-stats#deploy-on-your-own)를 참조하세요.

## 입력

- `card` (필수): 생성할 카드 유형. 지원: `stats`, `top-langs`, `pin`, `wakatime`, `gist`.
- `options`: 쿼리 문자열(`key=value&...`) 또는 JSON 형식의 카드 옵션. `username`이 생략되면 action은 저장소 소유자를 사용합니다.
- `path`: SVG 파일의 출력 경로. 기본값: `profile/<card>.svg`.
- `token`: GitHub 토큰(PAT 또는 `GITHUB_TOKEN`). 비공개 저장소 통계의 경우 `repo` 및 `read:user` 범위가 있는 [PAT](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)를 사용하세요.
- `version`: 사용할 github-readme-stats 바이너리 버전(예: `v1.0.0`). 기본값: `v1.0.0`. 최신 버전을 얻으려면 `latest`를 사용하세요.
- `repo`: `owner/repo` 형식의 GitHub 저장소. 기본값: `soulteary/github-readme-stats`.

## 출력

- `path`: SVG 파일이 작성된 경로.

## 옵션 매개변수

`options` 입력은 카드 유형에 따라 다른 매개변수를 허용합니다:

### Stats 카드 매개변수

- `username` (필수) - GitHub 사용자 이름
- `hide` - 특정 통계 숨기기 (쉼표로 구분, 예: `stars,commits`)
- `hide_title` - 제목 숨기기
- `hide_border` - 테두리 숨기기
- `hide_rank` - 순위 숨기기
- `show_icons` - 아이콘 표시
- `include_all_commits` - 모든 커밋 포함
- `theme` - 테마 이름 (80+ 테마 사용 가능)
- `bg_color` - 배경색 (16진수)
- `title_color` - 제목 색상
- `text_color` - 텍스트 색상
- `icon_color` - 아이콘 색상
- `border_color` - 테두리 색상
- `border_radius` - 테두리 반경
- `locale` - 언어 코드 (예: `zh`, `en`, `de`, `it`, `kr`, `ja`)
- `layout` - 레이아웃 유형 (`compact`, `normal`)

### Top Languages 카드 매개변수

- `username` (필수) - GitHub 사용자 이름
- `hide` - 특정 언어 숨기기 (쉼표로 구분)
- `layout` - 레이아웃 유형 (`compact`, `normal`)
- `langs_count` - 표시할 언어 수
- `theme` - 테마 이름
- `locale` - 언어 코드

### Pin 카드 매개변수

- `username` (필수) - GitHub 사용자 이름
- `repo` (필수) - 저장소 이름
- `theme` - 테마 이름
- `show_owner` - 소유자 표시
- `locale` - 언어 코드

### WakaTime 카드 매개변수

- `username` (필수) - WakaTime 사용자 이름
- `theme` - 테마 이름
- `hide` - 특정 통계 숨기기
- `layout` - 레이아웃 유형 (`compact`, `normal`)
- `langs_count` - 표시할 언어 수
- `hide_progress` - 진행 표시줄 숨기기
- `display_format` - 표시 형식 (`percent`, `time`)
- `locale` - 언어 코드

### Gist 카드 매개변수

- `id` (필수) - Gist ID
- `theme` - 테마 이름
- `locale` - 언어 코드

## 📖 사용 예제

이 action으로 만들 수 있는 몇 가지 예제입니다:

### GitHub Stats 카드

**기본:**

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![GitHub Stats](.github/assets/stats-basic.svg)

**다크 테마:**

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&theme=dark'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![GitHub Stats Dark](.github/assets/stats-dark.svg)

**컴팩트 레이아웃:**

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&layout=compact'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![GitHub Stats Compact](.github/assets/stats-compact.svg)

**아이콘 포함:**

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&show_icons=true'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![GitHub Stats With Icons](.github/assets/stats-icons.svg)

**사용자 정의 테마:**

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&bg_color=0d1117&title_color=ff6b6b&text_color=c9d1d9&border_color=30363d'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![GitHub Stats Custom](.github/assets/stats-custom.svg)

**순위 숨기기:**

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&hide_rank=true&show_icons=true'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![GitHub Stats Hide Rank](.github/assets/stats-hide-rank.svg)

### Repository Pin 카드

**기본:**

```yaml
- name: Generate pin card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: pin
    options: 'username=${{ github.repository_owner }}&repo=github-readme-stats'
    path: profile/pin.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Pinned Repo](.github/assets/repo-basic.svg)

**테마 적용:**

```yaml
- name: Generate pin card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: pin
    options: 'username=${{ github.repository_owner }}&repo=github-readme-stats&theme=dark'
    path: profile/pin.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Pinned Repo Themed](.github/assets/repo-themed.svg)

**소유자 표시:**

```yaml
- name: Generate pin card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: pin
    options: 'username=${{ github.repository_owner }}&repo=github-readme-stats&show_owner=true'
    path: profile/pin.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Pinned Repo With Owner](.github/assets/repo-owner.svg)

### Top Languages 카드

**기본:**

```yaml
- name: Generate top languages card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: top-langs
    options: 'username=${{ github.repository_owner }}'
    path: profile/top-langs.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Top Languages](.github/assets/top-langs-basic.svg)

**컴팩트 레이아웃:**

```yaml
- name: Generate top languages card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: top-langs
    options: 'username=${{ github.repository_owner }}&layout=compact&langs_count=6'
    path: profile/top-langs.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Top Languages Compact](.github/assets/top-langs-compact.svg)

**테마 적용:**

```yaml
- name: Generate top languages card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: top-langs
    options: 'username=${{ github.repository_owner }}&theme=radical&langs_count=8'
    path: profile/top-langs.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Top Languages Themed](.github/assets/top-langs-themed.svg)

**특정 언어 숨기기:**

```yaml
- name: Generate top languages card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: top-langs
    options: 'username=${{ github.repository_owner }}&hide=html,css,scss'
    path: profile/top-langs.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Top Languages Hide](.github/assets/top-langs-hide.svg)

### Gist 카드

**기본:**

```yaml
- name: Generate gist card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: gist
    options: 'id=bbfce31e0217a3689c8d961a356cb10d'
    path: profile/gist.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Gist](.github/assets/gist-basic.svg)

**테마 적용:**

```yaml
- name: Generate gist card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: gist
    options: 'id=bbfce31e0217a3689c8d961a356cb10d&theme=dark'
    path: profile/gist.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![Gist Themed](.github/assets/gist-themed.svg)

### WakaTime 카드

**기본:**

```yaml
- name: Generate wakatime card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: wakatime
    options: 'username=yourname'
    path: profile/wakatime.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![WakaTime Basic](.github/assets/wakatime-basic.svg)

**컴팩트 레이아웃:**

```yaml
- name: Generate wakatime card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: wakatime
    options: 'username=yourname&layout=compact'
    path: profile/wakatime.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![WakaTime Compact](.github/assets/wakatime-compact.svg)

**테마 적용:**

```yaml
- name: Generate wakatime card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: wakatime
    options: 'username=yourname&theme=radical&langs_count=5'
    path: profile/wakatime.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![WakaTime Themed](.github/assets/wakatime-themed.svg)

**진행 표시줄 숨기기:**

```yaml
- name: Generate wakatime card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: wakatime
    options: 'username=yourname&hide_progress=true'
    path: profile/wakatime.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

![WakaTime Hide Progress](.github/assets/wakatime-hide-progress.svg)

### JSON 옵션

옵션에 JSON 형식도 사용할 수 있습니다:

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: '{"username":"${{ github.repository_owner }}","show_icons":true,"hide_rank":true,"theme":"dark"}'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

### 버전 지정

사용할 바이너리의 특정 버전을 지정할 수 있습니다:

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&show_icons=true'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
    version: v1.0.0  # 특정 버전 사용
    # version: latest  # 또는 최신 버전 사용
```

## 작동 방식

이 action은 다음 방식으로 작동합니다:

1. **플랫폼 감지**: 운영 체제(Linux/macOS) 및 아키텍처(amd64/arm64) 자동 감지
2. **바이너리 다운로드**: 지정된 버전에 대한 GitHub Releases에서 사전 컴파일된 바이너리 다운로드
3. **CLI 호출**: 제공된 옵션으로 Go 바이너리의 CLI 모드 호출
4. **파일 저장**: 생성된 SVG를 지정된 경로에 작성

## 원본 버전과의 차이점

| 기능 | 원본 버전 | 이 버전 |
|------|---------|--------|
| 구현 | Node.js | Bash |
| 서비스 호출 | 직접 라이브러리 함수 호출 | Go 바이너리에 CLI 호출 |
| 종속성 | Node.js + npm 패키지 | curl(사전 설치됨) |
| 빌드 | npm install | Releases에서 다운로드 |
| 바이너리 소스 | npm 패키지 | GitHub Releases |

## 지원 플랫폼

- Linux (amd64, arm64)
- macOS (amd64, arm64)

Action은 자동으로 runner의 플랫폼을 감지하고 적절한 바이너리를 다운로드합니다.

## 참고사항

- 이 action은 [soulteary/github-readme-stats](https://github.com/soulteary/github-readme-stats)와 동일한 렌더러 및 페처를 사용합니다.
- Go 환경이 필요하지 않습니다 - 바이너리는 사전 컴파일되어 Releases에서 다운로드됩니다.
- 서비스 바이너리는 action 실행 중 임시로 다운로드되고 실행됩니다.
- 최상의 성능을 위해 API 호출을 피하기 위해 `latest` 대신 버전을 지정하세요.

## 라이선스

MIT License
