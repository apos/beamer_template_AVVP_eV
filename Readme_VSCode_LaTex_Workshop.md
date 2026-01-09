# Arbeiten mit LaTeX + Beamer in VS Code (LaTeX Workshop)

Diese Anleitung richtet sich an Einsteiger und beschreibt, wie das
AVVP‑Beamer‑Template **ohne Overleaf**, lokal und reproduzierbar mit
**Visual Studio Code** und der Erweiterung **LaTeX Workshop** verwendet wird.

---

## Voraussetzungen

Auf dem System müssen installiert sein:

- **TeX Live 2024/2025**
  - inkl. `lualatex`, `latexmk`, `biber`
- **Visual Studio Code**
- VS Code Extension: **LaTeX Workshop** (James Yu)

Überprüfung im Terminal:

```
which lualatex
which latexmk
which biber
```

---

## Projektstruktur (relevant)

```
AVVP_Beamer/
├── AVVP_VORLAGE_BEAMER.tex   ← Root-Dokument
├── tex/
│   └── avvp_beamer_preamble.tex
├── media/
├── pics/
├── bibtex.bib
├── .vscode/
│   └── settings.json
```

---

## Wichtiges Konzept: Root-Datei

In diesem Projekt ist **AVVP_VORLAGE_BEAMER.tex** das **Root-Dokument**.

Am Dateianfang stehen bewusst diese Magic Comments:

```
% !TEX root = AVVP_VORLAGE_BEAMER.tex
% !TEX program = lualatex
```

Sie stellen sicher, dass:

- immer die richtige Datei gebaut wird
- immer **LuaLaTeX** verwendet wird
- das Projekt auch für andere Nutzer stabil funktioniert

👉 Diese Kommentare sind **absichtlich zusätzlich** zur `settings.json`
vorhanden.

---

## Build-Prozess (automatisch)

Der Build erfolgt über **latexmk** mit LuaLaTeX und biber:

```
lualatex → biber → lualatex → lualatex
```

Das ist notwendig für:

- `biblatex`
- `csquotes`
- Zitate, Referenzen, Overlays

Die Steuerung erfolgt über:

```
.vscode/settings.json
```

Dort ist u. a. definiert:

- Auto-Build beim Speichern
- Ausgabe in `tex/tmp/`
- Viewer im VS‑Code‑Tab

---

## Kompilieren in VS Code

Empfohlener Ablauf:

1. **AVVP_VORLAGE_BEAMER.tex öffnen**
2. Datei speichern (`Cmd+S`)
3. PDF öffnet sich automatisch im VS‑Code‑Tab

Alternativ manuell:

- Command Palette →  
  **LaTeX Workshop: Build LaTeX project**

---

## Wichtiger Hinweis für Einsteiger

Wenn gerade eine andere `.tex`‑Datei geöffnet ist
(z. B. `avvp_beamer_preamble.tex`), kann LaTeX Workshop **nicht wissen**,
dass diese nicht das Hauptdokument ist.

➡ Lösung:
- Immer das Hauptdokument öffnen **oder**
- auf die Magic Comments vertrauen (empfohlen)

---

## Warum VS Code + LaTeX Workshop (statt Overleaf)

- Keine Compile‑Limits
- Volle Kontrolle über LuaLaTeX, Fonts, Bibliographie
- Reproduzierbar und versionskontrollierbar
- Gleiche Basis für **LyX‑Export** und **reines LaTeX**

Dieses Setup entspricht funktional einem lokalen „Overleaf‑Ersatz“.

---

## Troubleshooting

**Es passiert nichts beim Kompilieren**
- Prüfen, ob das Root‑Dokument aktiv ist
- Command Palette → *Build LaTeX project*

**Bibliographie fehlt**
- Prüfen, ob `biber` installiert ist
- Einmal vollständig kompilieren lassen (nicht abbrechen)

**Hilfsdateien liegen überall**
- Alle temporären Dateien landen bewusst in `tex/tmp/`
- Das Verzeichnis ist für `.gitignore` vorgesehen

---

## Empfehlung

Für Präsentationen gilt:

- **Inhalt** in LyX oder LaTeX
- **Design & Logik** im AVVP‑Theme
- **Build** über VS Code + LaTeX Workshop

Das ist die stabilste Kombination für Einsteiger und Fortgeschrittene.