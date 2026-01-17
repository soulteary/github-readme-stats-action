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

Generieren Sie [GitHub Readme Stats](https://github.com/soulteary/github-readme-stats) Karten in Ihrem GitHub Actions Workflow, committen Sie sie in Ihr Profil-Repository und betten Sie sie direkt von dort ein.

Diese Action verwendet die Go-Implementierung des `github-readme-stats` Dienstes, lädt vorkompilierte Binärdateien von GitHub Releases herunter und ruft sie über CLI auf, um Statistik-Karten zu generieren.

## Schnellstart

```yaml
name: Update README cards

on:
  schedule:
    - cron: "0 0 * * *" # Läuft täglich um Mitternacht
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

Dann in Ihrem Profil-README einbetten:

```md
![Stats](./profile/stats.svg)
![Top Languages](./profile/top-langs.svg)
![Pinned](./profile/pin-github-readme-stats.svg)
```

## Deployment-Optionen

Diese Action ist eine empfohlene Deployment-Option. Sie können auch auf Vercel oder anderen Plattformen deployen. Siehe das [GitHub Readme Stats README](https://github.com/soulteary/github-readme-stats#deploy-on-your-own).

## Eingaben

- `card` (erforderlich): Kartentyp. Unterstützt: `stats`, `top-langs`, `pin`, `wakatime`, `gist`.
- `options`: Optionen für die Karte als Query-String (`key=value&...`) oder JSON. Wenn `username` weggelassen wird, verwendet die Action den Repository-Besitzer.
- `path`: Ausgabepfad für die SVG-Datei. Standard: `profile/<card>.svg`.
- `token`: GitHub Token (PAT oder `GITHUB_TOKEN`). Für private Repository-Statistiken verwenden Sie ein [PAT](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) mit `repo` und `read:user` Berechtigungen.
- `version`: Version der github-readme-stats Binärdatei (z.B. `v1.0.0`). Standard: `v1.0.0`. Verwenden Sie `latest` für die neueste Version.
- `repo`: GitHub Repository im Format `owner/repo`. Standard: `soulteary/github-readme-stats`.

## Ausgaben

- `path`: Pfad, wo die SVG-Datei geschrieben wurde.

## Optionsparameter

Die `options` Eingabe akzeptiert je nach Kartentyp unterschiedliche Parameter:

### Stats Kartenparameter

- `username` (erforderlich) - GitHub Benutzername
- `hide` - Bestimmte Statistiken ausblenden (kommagetrennt, z.B. `stars,commits`)
- `hide_title` - Titel ausblenden
- `hide_border` - Rahmen ausblenden
- `hide_rank` - Rang ausblenden
- `show_icons` - Symbole anzeigen
- `include_all_commits` - Alle Commits einbeziehen
- `theme` - Themenname (80+ Themen verfügbar)
- `bg_color` - Hintergrundfarbe (hexadezimal)
- `title_color` - Titel Farbe
- `text_color` - Textfarbe
- `icon_color` - Symbolfarbe
- `border_color` - Rahmenfarbe
- `border_radius` - Rahmenradius
- `locale` - Sprachcode (z.B. `zh`, `en`, `de`, `it`, `kr`, `ja`)
- `layout` - Layout-Typ (`compact`, `normal`)

### Top Languages Kartenparameter

- `username` (erforderlich) - GitHub Benutzername
- `hide` - Bestimmte Sprachen ausblenden (kommagetrennt)
- `layout` - Layout-Typ (`compact`, `normal`)
- `langs_count` - Anzahl der anzuzeigenden Sprachen
- `theme` - Themenname
- `locale` - Sprachcode

### Pin Kartenparameter

- `username` (erforderlich) - GitHub Benutzername
- `repo` (erforderlich) - Repository-Name
- `theme` - Themenname
- `show_owner` - Besitzer anzeigen
- `locale` - Sprachcode

### WakaTime Kartenparameter

- `username` (erforderlich) - WakaTime Benutzername
- `theme` - Themenname
- `hide` - Bestimmte Statistiken ausblenden
- `layout` - Layout-Typ (`compact`, `normal`)
- `langs_count` - Anzahl der anzuzeigenden Sprachen
- `hide_progress` - Fortschrittsbalken ausblenden
- `display_format` - Anzeigeformat (`percent`, `time`)
- `locale` - Sprachcode

### Gist Kartenparameter

- `id` (erforderlich) - Gist ID
- `theme` - Themenname
- `locale` - Sprachcode

## 📖 Verwendungsbeispiele

Hier sind einige Beispiele für das, was Sie mit dieser Action erstellen können:

### GitHub Stats Karte

**Basis:**

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

**Dunkles Thema:**

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

**Kompaktes Layout:**

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

**Mit Symbolen:**

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

**Benutzerdefiniertes Thema:**

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

**Rang ausblenden:**

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

### Repository Pin Karte

**Basis:**

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

**Mit Thema:**

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

**Mit Besitzer:**

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

### Top Languages Karte

**Basis:**

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

**Kompaktes Layout:**

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

**Mit Thema:**

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

**Bestimmte Sprachen ausblenden:**

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

### Gist Karte

**Basis:**

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

**Mit Thema:**

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

### WakaTime Karte

**Basis:**

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

**Kompaktes Layout:**

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

**Mit Thema:**

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

**Fortschrittsbalken ausblenden:**

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

### JSON Optionen

Sie können auch JSON-Format für Optionen verwenden:

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: '{"username":"${{ github.repository_owner }}","show_icons":true,"hide_rank":true,"theme":"dark"}'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

### Version Angeben

Sie können eine spezifische Version der zu verwendenden Binärdatei angeben:

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&show_icons=true'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
    version: v1.0.0  # Verwenden Sie spezifische Version
    # version: latest  # Oder verwenden Sie neueste Version
```

## Funktionsweise

Diese Action funktioniert durch:

1. **Plattform-Erkennung**: Automatische Erkennung des Betriebssystems (Linux/macOS) und der Architektur (amd64/arm64)
2. **Binärdatei herunterladen**: Herunterladen der vorkompilierten Binärdatei von GitHub Releases für die angegebene Version
3. **CLI aufrufen**: Aufruf des Go-Binärdatei CLI-Modus mit den bereitgestellten Optionen
4. **Datei speichern**: Schreiben des generierten SVG in den angegebenen Pfad

## Unterschiede zur Originalversion

| Funktion | Originalversion | Diese Version |
|---------|----------------|---------------|
| Implementierung | Node.js | Bash |
| Service-Aufruf | Direkter Bibliotheksfunktionsaufruf | CLI-Aufruf an Go-Binärdatei |
| Abhängigkeiten | Node.js + npm Paket | curl (vorinstalliert) |
| Build | npm install | Von Releases herunterladen |
| Binärdatei-Quelle | npm Paket | GitHub Releases |

## Unterstützte Plattformen

- Linux (amd64, arm64)
- macOS (amd64, arm64)

Die Action erkennt automatisch die Plattform Ihres Runners und lädt die entsprechende Binärdatei herunter.

## Hinweise

- Diese Action verwendet dieselben Renderer und Fetcher wie [soulteary/github-readme-stats](https://github.com/soulteary/github-readme-stats).
- Keine Go-Umgebung erforderlich - Binärdateien sind vorkompiliert und werden von Releases heruntergeladen.
- Die Service-Binärdatei wird während der Action-Ausführung temporär heruntergeladen und ausgeführt.
- Für beste Leistung geben Sie eine Version an, anstatt `latest` zu verwenden, um API-Aufrufe zu vermeiden.

## Lizenz

MIT License
