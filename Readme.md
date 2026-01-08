# AVVP Beamer Theme (Beamer + LyX) – README

Dieses Dokument beschreibt **Aufbau, Struktur und Wirkungsweise** der AVVP‑Beamer‑Präambel.
Es erklärt **was wo definiert ist** und **welchen Effekt** Abschnitte, Unterabschnitte, Overlays,
Header, Footer und Agenda‑Frames haben – insbesondere im Zusammenspiel mit **LyX**.

---

## Quick-Start

LyX (empfohlen)
- Dokumentklasse: Beamer
- Präambel: `avvp_beamer_preamble.tex` einbinden (LyX: Dokument -> Einstellungen -> LaTeX-Präambel)
- Bibliographie: BibLaTeX aktivieren, Prozessor: **biber**
- Kompilieren (LyX): LuaLaTeX/XeLaTeX + biber (LyX übernimmt die Aufrufreihenfolge)

Reines LaTeX (ohne LyX)
- In dein Beamer-Dokument in der Präambel einbinden:
  - `\input{avvp_beamer_preamble.tex}`
- Kompilieren (Shell): `lualatex → biber → lualatex ×2`

Build-Commands (CLI)
```bash
lualatex AVVP_VORLAGE_BEAMER.tex
biber    AVVP_VORLAGE_BEAMER
lualatex AVVP_VORLAGE_BEAMER.tex
lualatex AVVP_VORLAGE_BEAMER.tex
```

Wichtig: Dieses Theme setzt auf **BibLaTeX + biber**. `bibtex` wird nicht unterstützt.

https://www.lyx.org/Home
<img width="822" height="242" alt="image" src="https://github.com/user-attachments/assets/b969f1b3-8aeb-4bda-b655-b01c69cc5b3c" />



---

## 1. Grundidee des AVVP‑Themes

Ziele des Themes:

- Einheitliches Erscheinungsbild für AVVP‑Vorträge
- Klare Trennung von Inhalt und Gestaltung
- Volle LyX‑Kompatibilität (keine manuellen Frame‑Hacks im Dokument)
- Kontrollierte Nutzung von:
  - Dark / Light Mode
  - Overlays
  - Agenda‑Zwischenfolien
  - Footer mit Autoren / Kurztitel / Logo

Grundprinzip:

> Beamer‑Automatik wird nur dort verwendet, wo sie **vorhersagbar** ist.
> Kritische Bereiche (Logo, Agenda, Overlays) werden **explizit gesteuert**.


<table>
  <tr>
    <td>
      <a href="https://github.com/user-attachments/assets/12af362e-6773-4e8a-9feb-7df1aa42300c">
        <img src="https://github.com/user-attachments/assets/12af362e-6773-4e8a-9feb-7df1aa42300c" width="100%" />
      </a>
    </td>
    <td>
      <a href="https://github.com/user-attachments/assets/9df4c369-3747-4960-aa12-ad97c4801909">
        <img src="https://github.com/user-attachments/assets/9df4c369-3747-4960-aa12-ad97c4801909" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/user-attachments/assets/bb2ce8f2-4f5d-487c-92e0-36a0cf115f45">
        <img src="https://github.com/user-attachments/assets/bb2ce8f2-4f5d-487c-92e0-36a0cf115f45" width="100%" />
      </a>
    </td>
    <td>
      <a href="https://github.com/user-attachments/assets/69bb8994-7648-426c-98cc-93f2039a1161">
        <img src="https://github.com/user-attachments/assets/69bb8994-7648-426c-98cc-93f2039a1161" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/user-attachments/assets/787c11bf-7f71-4809-8046-8780e3c71651">
        <img src="https://github.com/user-attachments/assets/787c11bf-7f71-4809-8046-8780e3c71651" width="100%" />
      </a>
    </td>
    <td>
      <a href="https://github.com/user-attachments/assets/e9c395b6-a03c-46bf-8607-126d8ed13dda">
        <img src="https://github.com/user-attachments/assets/e9c395b6-a03c-46bf-8607-126d8ed13dda" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td colspan="2">
      <a href="https://github.com/user-attachments/assets/9078de16-3764-42e4-971b-153eff7fb8f0">
        <img src="https://github.com/user-attachments/assets/9078de16-3764-42e4-971b-153eff7fb8f0" width="100%" />
      </a>
    </td>
  </tr>
</table>

## 1a. BibLaTeX, biber und Bildnachweise

Dieses Theme setzt **konsequent auf BibLaTeX mit biber** als Backend.

Warum?
- Einheitliche Verwaltung von Literatur- **und Bildnachweisen**
- Saubere Trennung zwischen Inhalt, Zitierlogik und Darstellung
- Bessere Kontrolle über Sortierung/Formatierung als mit klassischem BibTeX

Voraussetzungen
- LaTeX-Engine: **LuaLaTeX** (empfohlen) oder XeLaTeX
- Bibliographie-Backend: **biber**
- Paket: `biblatex`

Wichtig
- `bibtex` wird **nicht** unterstützt
- Für korrekte Ergebnisse ist die Build-Kette nötig: `lualatex → biber → lualatex ×2`

Typische Konfiguration
```latex
\usepackage[backend=biber,style=authoryear]{biblatex}
\addbibresource{bib/references.bib}
\addbibresource{bib/images.bib}
```

Das Theme nutzt BibLaTeX auch für
- Bildnachweise (Overlays, Hintergrundbilder)
- konsistente Quellenangaben über alle Folien hinweg

---

## 2. Farbsystem

```latex
\definecolor{AVVPBg}{RGB}{5,23,41}        % Haupt-Hintergrund (Dark)
\definecolor{AVVPBlue}{RGB}{46,108,198}   % Überschriften / Struktur
\definecolor{AVVPSpark}{RGB}{235,156,70}  % Hervorhebung / aktiv
\definecolor{AVVPGrey}{RGB}{90,98,110}    % Gedimmt / sekundär
```

| Farbe | Verwendung |
|------|-----------|
| AVVPBg | Dark‑Hintergrund, Footer |
| AVVPBlue | Titel, Abschnittsüberschriften |
| AVVPSpark | aktive Punkte, Overlays |
| AVVPGrey | inaktive / gedimmte Inhalte |

---

## 3. Dark‑ und Light‑Mode

### Aktivierung

```latex
\AVVPDarkMode
\AVVPLightMode
```

Diese Makros setzen **global**:

- Hintergrund
- Textfarbe
- Listenfarben
- Overlay‑Transparenz
- TOC‑Farben

### Overlay‑Dimmung

```latex
\setbeamercovered{transparent=45}
```

Je höher der Wert, desto stärker die Dimmung.

---

## 4. Bulletpoints & Nummerierungen

### Sparkle‑Bullets

- Eigene TikZ‑Bullets
- Robust definiert
- Ersetzen alle Standard‑Beamer‑Bullets

```latex
\setbeamertemplate{itemize item}{\avvpsparkbullet}
```

### Nummerierungen

```latex
\setbeamercolor{enumerate item}{fg=AVVPSpark}
\setbeamerfont{enumerate item}{series=\bfseries}
```

---

## 5. Overlays

Globale Overlay‑Logik:

```latex
\beamerdefaultoverlayspecification{<+-|alert@+>}
```

- Aktives Element → `alerted text` (AVVPSpark)
- Nicht‑aktive → transparent gedimmt

```latex
\setbeamercolor{alerted text}{fg=AVVPSpark}
```

---

## 6. Header (Malmoe)

- Layout unverändert (Malmoe)
- Nur Farben überschrieben

| Ebene | Farbe |
|-----|------|
| Abschnitt | Blau |
| Unterabschnitt (aktiv) | Orange |
| Unterabschnitt (inaktiv) | Grau |
| Aktiver Unterabschnitt | zusätzlich ▶‑Marker |

---

## 7. Agenda / Zwischenagenda

### Automatisch bei jedem Unterabschnitt

```latex
\AtBeginSubsection[]{ \AVVPAgendaFrame }
```

Kein manuelles Einfügen nötig.

### Darstellung

- Titel: „Weiter geht’s im Text …“
- Hintergrund: AVVPBg
- Abschnitte: Blau
- Unterabschnitte:
  - aktuell: Orange + ▶
  - andere: Weiß

---

## 8. Footer

### Aufbau

| Position | Inhalt |
|-------|-------|
| Links | Autoren (`\insertshortauthor`) |
| Mitte | Kurztitel (`\insertshorttitle`) |
| Rechts | **nur Logo**, Hintergrund AVVPBg |

### Logo – zentrale Definition

```latex
\pgfdeclareimage[height=2.2ex]{avvp-logo}{pics/avvp_2019_logo_wortmarke_neg.png}
\newcommand{\AVVPLogo}{\pgfuseimage{avvp-logo}}
```

Beamer‑Standardlogo ist deaktiviert:

```latex
\setbeamertemplate{logo}{}
```

Logo erscheint **nur**, wo `\AVVPLogo` explizit verwendet wird.

---

## 9. Titelfolie (LyX)

Ziel:

- Keine Headline
- Kein Footer
- Keine Seitennummer

Umsetzung (LyX‑sicher):

```latex
\makeatletter
\@ifundefined{makebeamertitle}{}{%
  \renewcommand{\makebeamertitle}{%
    \begin{frame}[plain,noframenumbering]
      \titlepage
    \end{frame}
  }%
}
\makeatother
```

---

## 10. Wichtige Regeln

- Logo **nie** über `\logo{}` anzeigen
- Agenda **nur automatisch**
- Overlays **global**, nicht pro Liste
- Dark / Light Mode **nur über Makros**

---

## 11. Status

✔ LyX‑stabil  
✔ Dark / Light sauber getrennt  
✔ Header / Footer kontrolliert  

**AVVP Beamer Theme – Stand 01-2026**

## 11a. Verwendung ohne LyX (reines LaTeX / Beamer)

Das AVVP‑Beamer‑Theme ist **nicht an LyX gebunden** und kann in jedes Beamer‑Projekt übernommen werden.

Grundprinzip (wichtig für die Wartbarkeit):
- Das gesamte Theme lebt in der **Präambel**
- Es werden **keine LyX‑spezifischen Makros** vorausgesetzt
- Jeder bestehende Beamer‑Inhalt kann unverändert weiterverwendet

Das bedeutet:
- Vorhandene LaTeX‑Beamer‑Präsentationen können das Theme direkt übernehmen
- Inhalte (`frame`, `itemize`, `equation`, `figure`, …) bleiben unverändert
- Nur die Präambel wird ersetzt oder ergänzt

Hinweis: Das Theme ist so aufgebaut, dass es sich als **reiner Präambel‑Block** nutzen lässt. Du kannst Inhalte aus beliebigen bestehenden Präsentationen übernehmen, ohne sie umzuschreiben.

Minimalbeispiel (ohne LyX):
```latex
\documentclass{beamer}

% AVVP‑Theme‑Präambel einbinden
\input{avvp_beamer_preamble.tex}

\title{Beispiel}
\author{Autor}
\date{\today}

\begin{document}

\begin{frame}{Ein Beispiel}
  \begin{itemize}
    \item Inhalt bleibt Standard‑Beamer
    \item Darstellung kommt aus dem Theme
  \end{itemize}
\end{frame}

\end{document}
```

LyX bietet zusätzlichen Komfort (Struktur, Schutz vor Syntaxfehlern),
ist aber **keine technische Voraussetzung**.

---

## 12. Lizenz

Dieses Beamer‑Template sowie alle zugehörigen Inhalte (LaTeX‑Präambel, Makros, Dokumentstruktur, Beispielgrafiken, sofern nicht anders gekennzeichnet) stehen unter der Lizenz:

**Creative Commons Attribution–NonCommercial–ShareAlike 4.0 International**  
(**CC BY‑NC‑SA 4.0**)

### Erlaubt
- Teilen (Kopieren und Weiterverbreiten in jedem Medium oder Format)
- Bearbeiten (Remixen, Verändern und Weiterentwickeln)

### Bedingungen
- **Namensnennung (BY)**  
  Angabe des Urhebers: *Astronomische Vereinigung Vorderpfalz e.V. (AVVP e.V.)*
- **Nicht kommerziell (NC)**  
  Keine Nutzung für kommerzielle Zwecke
- **Weitergabe unter gleichen Bedingungen (SA)**  
  Abgeleitete Werke müssen unter derselben Lizenz stehen

### Rechtstext
- Offizielle Lizenzseite:  
  https://creativecommons.org/licenses/by-nc-sa/4.0/
- Deutscher Lizenztext (unverbindliche Übersetzung):  
  https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode.de

**Hinweis:**  
Rechtlich verbindlich ist ausschließlich der englische Originaltext der Lizenz.
