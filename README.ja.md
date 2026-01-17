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

GitHub Actions ワークフローで [GitHub Readme Stats](https://github.com/soulteary/github-readme-stats) カードを生成し、プロフィールリポジトリにコミットして、そこから直接埋め込みます。

この Action は `github-readme-stats` サービスの Go 実装を使用し、GitHub Releases から事前コンパイルされたバイナリをダウンロードし、CLI 経由で呼び出して統計カードを生成します。

## クイックスタート

```yaml
name: Update README cards

on:
  schedule:
    - cron: "0 0 * * *" # 毎日深夜に1回実行
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

次に、プロフィール README から埋め込みます:

```md
![Stats](./profile/stats.svg)
![Top Languages](./profile/top-langs.svg)
![Pinned](./profile/pin-github-readme-stats.svg)
```

## デプロイオプション

この action は推奨されるデプロイオプションの1つです。Vercel やその他のプラットフォームでもデプロイできます。[GitHub Readme Stats README](https://github.com/soulteary/github-readme-stats#deploy-on-your-own) を参照してください。

## 入力

- `card` (必須): 生成するカードタイプ。サポート: `stats`, `top-langs`, `pin`, `wakatime`, `gist`。
- `options`: クエリ文字列(`key=value&...`)または JSON 形式のカードオプション。`username` が省略された場合、action はリポジトリ所有者を使用します。
- `path`: SVG ファイルの出力パス。デフォルト: `profile/<card>.svg`。
- `token`: GitHub トークン(PAT または `GITHUB_TOKEN`)。プライベートリポジトリの統計の場合、`repo` および `read:user` スコープを持つ [PAT](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) を使用してください。
- `version`: 使用する github-readme-stats バイナリのバージョン(例: `v1.0.0`)。デフォルト: `v1.0.0`。最新バージョンを取得するには `latest` を使用します。
- `repo`: `owner/repo` 形式の GitHub リポジトリ。デフォルト: `soulteary/github-readme-stats`。

## 出力

- `path`: SVG ファイルが書き込まれたパス。

## オプションパラメータ

`options` 入力は、カードタイプに応じて異なるパラメータを受け入れます：

### Stats カードパラメータ

- `username` (必須) - GitHub ユーザー名
- `hide` - 特定の統計を非表示（カンマ区切り、例：`stars,commits`）
- `hide_title` - タイトルを非表示
- `hide_border` - ボーダーを非表示
- `hide_rank` - ランクを非表示
- `show_icons` - アイコンを表示
- `include_all_commits` - すべてのコミットを含める
- `theme` - テーマ名（80+ テーマ利用可能）
- `bg_color` - 背景色（16進数）
- `title_color` - タイトル色
- `text_color` - テキスト色
- `icon_color` - アイコン色
- `border_color` - ボーダー色
- `border_radius` - ボーダー半径
- `locale` - 言語コード（例：`zh`、`en`、`de`、`it`、`kr`、`ja`）
- `layout` - レイアウトタイプ（`compact`、`normal`）

### Top Languages カードパラメータ

- `username` (必須) - GitHub ユーザー名
- `hide` - 特定の言語を非表示（カンマ区切り）
- `layout` - レイアウトタイプ（`compact`、`normal`）
- `langs_count` - 表示する言語数
- `theme` - テーマ名
- `locale` - 言語コード

### Pin カードパラメータ

- `username` (必須) - GitHub ユーザー名
- `repo` (必須) - リポジトリ名
- `theme` - テーマ名
- `show_owner` - 所有者を表示
- `locale` - 言語コード

### WakaTime カードパラメータ

- `username` (必須) - WakaTime ユーザー名
- `theme` - テーマ名
- `hide` - 特定の統計を非表示
- `layout` - レイアウトタイプ（`compact`、`normal`）
- `langs_count` - 表示する言語数
- `hide_progress` - プログレスバーを非表示
- `display_format` - 表示形式（`percent`、`time`）
- `locale` - 言語コード

### Gist カードパラメータ

- `id` (必須) - Gist ID
- `theme` - テーマ名
- `locale` - 言語コード

## 📖 使用例

この action で作成できる例をいくつか示します：

### GitHub Stats カード

**基本:**

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

**ダークテーマ:**

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

**コンパクトレイアウト:**

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

**アイコン付き:**

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

**カスタムテーマ:**

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

**ランク非表示:**

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

### Repository Pin カード

**基本:**

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

**テーマ適用:**

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

**所有者表示:**

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

### Top Languages カード

**基本:**

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

**コンパクトレイアウト:**

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

**テーマ適用:**

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

**特定言語非表示:**

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

### Gist カード

**基本:**

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

**テーマ適用:**

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

### WakaTime カード

**基本:**

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

**コンパクトレイアウト:**

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

**テーマ適用:**

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

**プログレスバー非表示:**

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

### JSON オプション

オプションに JSON 形式も使用できます：

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: '{"username":"${{ github.repository_owner }}","show_icons":true,"hide_rank":true,"theme":"dark"}'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

### バージョン指定

使用するバイナリの特定のバージョンを指定できます：

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&show_icons=true'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
    version: v1.0.0  # 特定のバージョンを使用
    # version: latest  # または最新バージョンを使用
```

## 動作方法

この action は次のように動作します:

1. **プラットフォーム検出**: オペレーティングシステム(Linux/macOS)とアーキテクチャ(amd64/arm64)を自動検出
2. **バイナリダウンロード**: 指定されたバージョンの GitHub Releases から事前コンパイルされたバイナリをダウンロード
3. **CLI 呼び出し**: 提供されたオプションで Go バイナリの CLI モードを呼び出し
4. **ファイル保存**: 生成された SVG を指定されたパスに書き込み

## オリジナルバージョンとの違い

| 機能 | オリジナルバージョン | このバージョン |
|------|-------------------|--------------|
| 実装 | Node.js | Bash |
| サービス呼び出し | 直接ライブラリ関数呼び出し | Go バイナリへの CLI 呼び出し |
| 依存関係 | Node.js + npm パッケージ | curl(事前インストール済み) |
| ビルド | npm install | Releases からダウンロード |
| バイナリソース | npm パッケージ | GitHub Releases |

## サポートプラットフォーム

- Linux (amd64, arm64)
- macOS (amd64, arm64)

Action は自動的に runner のプラットフォームを検出し、適切なバイナリをダウンロードします。

## 注意事項

- この action は [soulteary/github-readme-stats](https://github.com/soulteary/github-readme-stats) と同じレンダラーとフェッチャーを使用します。
- Go 環境は不要です - バイナリは事前コンパイルされ、Releases からダウンロードされます。
- サービスバイナリは action 実行中に一時的にダウンロードされ実行されます。
- 最高のパフォーマンスを得るには、API 呼び出しを避けるために `latest` の代わりにバージョンを指定してください。

## ライセンス

MIT License
