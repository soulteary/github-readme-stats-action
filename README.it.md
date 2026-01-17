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

Genera carte [GitHub Readme Stats](https://github.com/soulteary/github-readme-stats) nel tuo workflow GitHub Actions, committale nel tuo repository del profilo e incorporale direttamente da lì.

Questa Action utilizza l'implementazione Go del servizio `github-readme-stats`, scarica binari precompilati da GitHub Releases e li chiama tramite CLI per generare carte statistiche.

## Guida Rapida

```yaml
name: Update README cards

on:
  schedule:
    - cron: "0 0 * * *" # Esegue una volta al giorno a mezzanotte
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

Quindi incorpora dal tuo README del profilo:

```md
![Stats](./profile/stats.svg)
![Top Languages](./profile/top-langs.svg)
![Pinned](./profile/pin-github-readme-stats.svg)
```

## Opzioni di Deployment

Questa action è un'opzione di deployment consigliata. Puoi anche deployare su Vercel o altre piattaforme. Vedi il [README di GitHub Readme Stats](https://github.com/soulteary/github-readme-stats#deploy-on-your-own).

## Input

- `card` (richiesto): Tipo di carta. Supportati: `stats`, `top-langs`, `pin`, `wakatime`, `gist`.
- `options`: Opzioni per la carta come stringa di query (`key=value&...`) o JSON. Se `username` viene omesso, l'action utilizza il proprietario del repository.
- `path`: Percorso di output per il file SVG. Predefinito: `profile/<card>.svg`.
- `token`: Token GitHub (PAT o `GITHUB_TOKEN`). Per statistiche di repository private, usa un [PAT](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) con scope `repo` e `read:user`.
- `version`: Versione del binario github-readme-stats da utilizzare (es. `v1.0.0`). Predefinito: `v1.0.0`. Usa `latest` per ottenere l'ultima versione.
- `repo`: Repository GitHub nel formato `owner/repo`. Predefinito: `soulteary/github-readme-stats`.

## Output

- `path`: Percorso dove è stato scritto il file SVG.

## Parametri Opzioni

L'input `options` accetta parametri diversi a seconda del tipo di carta:

### Parametri Carta Stats

- `username` (richiesto) - Nome utente GitHub
- `hide` - Nascondi statistiche specifiche (separate da virgola, es. `stars,commits`)
- `hide_title` - Nascondi titolo
- `hide_border` - Nascondi bordo
- `hide_rank` - Nascondi classifica
- `show_icons` - Mostra icone
- `include_all_commits` - Includi tutti i commit
- `theme` - Nome tema (80+ temi disponibili)
- `bg_color` - Colore di sfondo (esadecimale)
- `title_color` - Colore titolo
- `text_color` - Colore testo
- `icon_color` - Colore icona
- `border_color` - Colore bordo
- `border_radius` - Raggio bordo
- `locale` - Codice lingua (es. `zh`, `en`, `de`, `it`, `kr`, `ja`)
- `layout` - Tipo layout (`compact`, `normal`)

### Parametri Carta Top Languages

- `username` (richiesto) - Nome utente GitHub
- `hide` - Nascondi lingue specifiche (separate da virgola)
- `layout` - Tipo layout (`compact`, `normal`)
- `langs_count` - Numero di lingue da mostrare
- `theme` - Nome tema
- `locale` - Codice lingua

### Parametri Carta Pin

- `username` (richiesto) - Nome utente GitHub
- `repo` (richiesto) - Nome repository
- `theme` - Nome tema
- `show_owner` - Mostra proprietario
- `locale` - Codice lingua

### Parametri Carta WakaTime

- `username` (richiesto) - Nome utente WakaTime
- `theme` - Nome tema
- `hide` - Nascondi statistiche specifiche
- `layout` - Tipo layout (`compact`, `normal`)
- `langs_count` - Numero di lingue da mostrare
- `hide_progress` - Nascondi barra di progresso
- `display_format` - Formato visualizzazione (`percent`, `time`)
- `locale` - Codice lingua

### Parametri Carta Gist

- `id` (richiesto) - ID Gist
- `theme` - Nome tema
- `locale` - Codice lingua

## 📖 Esempi di Utilizzo

Ecco alcuni esempi di ciò che puoi creare con questa action:

### Carta GitHub Stats

**Base:**

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

**Tema Scuro:**

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

**Layout Compatto:**

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

**Con Icone:**

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

**Tema Personalizzato:**

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

**Nascondi Classifica:**

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

### Carta Repository Pin

**Base:**

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

**Tematico:**

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

**Con Proprietario:**

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

### Carta Top Languages

**Base:**

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

**Layout Compatto:**

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

**Tematico:**

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

**Nascondi Lingue Specifiche:**

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

### Carta Gist

**Base:**

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

**Tematico:**

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

### Carta WakaTime

**Base:**

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

**Layout Compatto:**

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

**Tematico:**

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

**Nascondi Barra di Progresso:**

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

### Opzioni JSON

Puoi anche usare il formato JSON per le opzioni:

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: '{"username":"${{ github.repository_owner }}","show_icons":true,"hide_rank":true,"theme":"dark"}'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
```

### Specifica Versione

Puoi specificare una versione specifica del binario da utilizzare:

```yaml
- name: Generate stats card
  uses: soulteary/github-readme-stats-action@v1.0.0
  with:
    card: stats
    options: 'username=${{ github.repository_owner }}&show_icons=true'
    path: profile/stats.svg
    token: ${{ secrets.GITHUB_TOKEN }}
    version: v1.0.0  # Usa versione specifica
    # version: latest  # Oppure usa ultima versione
```

## Come Funziona

Questa action funziona:

1. **Rilevamento Piattaforma**: Rileva automaticamente il sistema operativo (Linux/macOS) e l'architettura (amd64/arm64)
2. **Download Binario**: Scarica il binario precompilato da GitHub Releases per la versione specificata
3. **Chiamata CLI**: Richiama la modalità CLI del binario Go con le opzioni fornite
4. **Salvataggio File**: Scrive l'SVG generato nel percorso specificato

## Differenze dalla Versione Originale

| Caratteristica | Versione Originale | Questa Versione |
|----------------|-------------------|-----------------|
| Implementazione | Node.js | Bash |
| Chiamata Servizio | Chiamata diretta funzione libreria | Chiamata CLI a binario Go |
| Dipendenze | Node.js + pacchetto npm | curl (preinstallato) |
| Build | npm install | Download da Releases |
| Sorgente Binario | pacchetto npm | GitHub Releases |

## Piattaforme Supportate

- Linux (amd64, arm64)
- macOS (amd64, arm64)

L'action rileva automaticamente la piattaforma del runner e scarica il binario appropriato.

## Note

- Questa action utilizza gli stessi renderer e fetcher di [soulteary/github-readme-stats](https://github.com/soulteary/github-readme-stats).
- Nessun ambiente Go richiesto - i binari sono precompilati e scaricati da Releases.
- Il binario del servizio viene temporaneamente scaricato ed eseguito durante l'esecuzione dell'action.
- Per le migliori prestazioni, specifica una versione invece di usare `latest` per evitare chiamate API.

## Licenza

MIT License
