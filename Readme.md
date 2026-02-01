# AVVP Beamer Theme (Beamer + LyX) – README


Dieses Dokument beschreibt **Aufbau, Struktur und Wirkungsweise** der AVVP‑Beamer‑Präambel.
Die Vorlage ist **für LyX optimiert**, funktioniert aber **uneingeschränkt auch als reines LaTeX‑Beamer‑Theme**.
Die gesamte Gestaltung ist in der Präambel gekapselt, sodass bestehende Beamer‑Dokumente ohne inhaltliche Änderungen übernommen werden können.

Urheber:
Astronomische Vereinigung Vorderpfalz e.V., kurz: AVVP e.V.,
https://avvp.de

Autor (2026):
Axel Pospischil, apos@blue-it.org, https://github.com/apos

Hilfsmittel: ChatGPG 5.x

<img width="601" height="308" alt="image" src="https://github.com/user-attachments/assets/64423d71-188a-4ae7-8eb2-78db50da9d4a" />

---

## Inhaltsverzeichnis

- [AVVP Beamer Theme (Beamer + LyX) – README](#avvp-beamer-theme-beamer--lyx--readme)
  - [Inhaltsverzeichnis](#inhaltsverzeichnis)
  - [Motivation](#motivation)
  - [Quick-Start](#quick-start)
  - [Wichtiger Hinweis: Programmlistings (LyX) und Beamer `fragile`](#wichtiger-hinweis-programmlistings-lyx-und-beamer-fragile)
    - [Arbeiten mit VS Code (LaTeX Workshop)](#arbeiten-mit-vs-code-latex-workshop)
    - [Prism (Online-Editor)](#prism-online-editor)
    - [Hinweis: LyX / biber Cache-Probleme](#hinweis-lyx--biber-cache-probleme)
  - [Grundidee des AVVP‑Themes](#grundidee-des-avvpthemes)
  - [BibLaTeX, biber und Bildnachweise](#biblatex-biber-und-bildnachweise)
  - [Farbsystem](#farbsystem)
  - [Design-Guidelines (CI)](#design-guidelines-ci)
  - [Schriftarten \& Typografie](#schriftarten--typografie)
  - [Dark‑ und Light‑Mode](#dark-und-lightmode)
    - [Aktivierung](#aktivierung)
    - [Overlay‑Dimmung](#overlaydimmung)
  - [Bulletpoints \& Nummerierungen](#bulletpoints--nummerierungen)
    - [Sparkle‑Bullets](#sparklebullets)
    - [Nummerierungen](#nummerierungen)
  - [Overlays](#overlays)
  - [Header (angepasst)](#header-angepasst)
    - [Agenda‑Zeile im Footer (oben) gezielt abschalten](#agendazeile-im-footer-oben-gezielt-abschalten)
  - [Zwischenagenda (automatisch)](#zwischenagenda-automatisch)
    - [Automatik](#automatik)
    - [Titel der Zwischenagenda](#titel-der-zwischenagenda)
    - [Robustheit / Overlays](#robustheit--overlays)
  - [Footer (CI‑gebunden)](#footer-cigebunden)
  - [Neue Makros (Kurzreferenz)](#neue-makros-kurzreferenz)
  - [Titelfolie (LyX)](#titelfolie-lyx)
  - [Status](#status)
  - [Verwendung ohne LyX (reines LaTeX / Beamer)](#verwendung-ohne-lyx-reines-latex--beamer)
  - [Lizenz](#lizenz)
    - [Erlaubt](#erlaubt)
    - [Bedingungen](#bedingungen)
    - [Rechtstext](#rechtstext)


## Motivation

Ein bisschen Nostalgie ist heutzutage schon dabei, eine Präsentation mit LaTeX zu machen. Natürlich haben wir im Verein auch Vorlagen für Powerpoint und insbesondere Google-Präsentation. Ich persönlich mag LaTeX, man fokussiert sich auf die Inhalte und nicht aufs Herumformatieren. Allerdings ist es für Nicht-Technikaffine Menschen gewöhnungsbedürtig und man kommt auch schnell in eine Fehlerhölle, aus der man nicht ohne weiteres wieder herauskommt. TeX ist eben letzten eine Programmiersprache und der Text wird "kompiliert". Da kann es Fehler geben. 

Hinzu kommt die Einschränkung bei der Darstellung von Multimedia-Inhalten. Ich habe entsprechende Makros eingefügt, die dabei helfen sollen. Wenn man seinen eigenen Rechner verwendet und volle Datei-Pfade angibt, dann wird ein Link im PDF erzeugt. Präsentiert man im Browser, ist das ganze dann auch problemlos für lokale Medien (Gif, MP4, ...) möglich. Nichtsdestotrotz sorgt man dafür, dass man in der ordentlichen Dateiablage die Sachen auch wiederfindet. Das gilt für jede Präsentation.

Besondern Wert habe ich auf die Refernezierung von Medien gelegt. Alle verwendeten Bilder, Quellen, Tabelle usw. erzeugen mit einem einfachen Macro eine aus dem Bibtex "cite-key" sowohl einen vernünftigen Hinweis auf der Folie, als auch gleichzeitig einen Eintrag im Inhaltsverzeichnis. Das ist sehr wertvoll und erspart eine Menge Arbeit. Alle Quellen werden ordentlich im Bibtex verwaltet. 

Dann viel Freude damit und beim Präsentieren interessanter astronomischer Themen. 

LG Axel
[https://avvp.de/](https://avvp.de/portfolio-view/axel-pospischil/)

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


## Wichtiger Hinweis: Programmlistings (LyX) und Beamer `fragile`

Wenn ein Frame in LyX ein **Programmlisting** (Paket `listings`, LyX-Inset „Program Listing“) enthält, muss der Frame in Beamer als **fragile** gebaut werden.

Hintergrund (warum das relevant ist):
- `listings` arbeitet intern wie „verbatim“.
- Beamer benötigt dafür die Option `fragile`, sonst kann die Frame-Verarbeitung abbrechen.
- Besonders kritisch: Wenn im Listing wörtlich Text wie `\end{frame}` vorkommt, kann Beamer den Frame sonst „vorzeitig“ beenden.

Praktische Regel (für LyX):
- Für jeden Frame mit Programmlisting im LyX-Frame-Inset **Frame-Eigenschaften → Fragile** aktivieren.

Beispiel (LaTeX-Export aus LyX):
```latex
\begin{frame}[fragile]{Quellcode}
  \begin{lstlisting}[language=TeX]
  ...
  \end{lstlisting}
\end{frame}
```

Hinweis:
- Diese Anforderung betrifft nur Frames, die `lstlisting`/Programmlistings enthalten.
- Normale Frames (ohne Listing) bleiben unverändert.

Wichtig: Dieses Theme setzt auf **BibLaTeX + biber**. `bibtex` wird nicht unterstützt.

### Arbeiten mit VS Code (LaTeX Workshop)

Neben LyX kann das AVVP‑Beamer‑Theme auch komfortabel mit **Visual Studio Code** verwendet werden.
Eine vollständige Schritt‑für‑Schritt‑Anleitung zur Einrichtung von **LaTeX Workshop**, LuaLaTeX, `latexmk` und `biber`
findet sich in der separaten Dokumentation:

👉 `Readme_VSCode_LaTex_Workshop.md`

Diese Variante eignet sich besonders für:
- Nutzer ohne LyX
- Git‑basierte Workflows
- Direkte Kontrolle über Build‑Kette und Artefakte

<a id="prism-online-editor"></a>

### Arbeiten mit Prism (KI Online-Editor)

Prism ist ein Online‑LaTeX‑Editor; diese Vorlage ist so aufgebaut, dass sie dort direkt kompiliert (LuaLaTeX + `biber`, lokale Schriften aus `fonts/`).

Prism:
```text
https://crixet.com/
```

- Öffne in Prism die gewünschte Hauptdatei (z. B. `AVVP_VORLAGE_BEAMER_MINIMAL.tex`) und starte die Kompilierung aus dem PDF‑Viewer.
- Das Theme benötigt **LuaLaTeX** (oder XeLaTeX) wegen `fontspec` und lokaler OTF/TTF‑Schriften.

**Wichtig (LyX‑Export → Prism):**
LyX schreibt die Präambel/`\input{tex/avvp_beamer_preamble.tex}` **nicht** ganz an den Dateianfang. Daher muss der LuaLaTeX‑Hinweis für Prism in der exportierten `.tex` **ganz oben** stehen:

```tex
% !TEX program = lualatex
```

Praktische Regel:
- Nach dem Export aus LyX diese Zeile als **erste Zeile** in die `.tex` einfügen (oder per kleinem Post‑Processing automatisieren).

---

### Hinweis: LyX / biber Cache-Probleme

Wenn `biblatex` / `biber` **ohne erkennbare Codeänderung** plötzlich nicht mehr funktioniert
(z. B. leere `.bbl`, fehlende Literatur trotz vorhandener `.bcf`, scheinbar „spontane“ Fehler),
handelt es sich in der Praxis häufig um ein **Zustands- oder Cacheproblem** von LyX.

Bewährter Fix:
- In LyX: **Tools → Reconfigure** ausführen
- LyX anschließend **vollständig neu starten**
- Falls nötig: LyX-Cache und temporäre Build-Verzeichnisse löschen  
  (u. a. unter `~/Library/Application Support/LyX-2.4/cache` sowie macOS-Temp-Verzeichnisse)

Hinweis:
Dieses Verhalten kann auch auftreten, **ohne dass sich TeX Live oder biber-Versionen geändert haben**.

Für Beipiele seht euch einfach die Beispiel-PDF an: https://github.com/apos/beamer_template_AVVP_eV/blob/main/AVVP_VORLAGE_BEAMER.pdf oder öffnet das LyX Dokument in Lyx. Das sollte eigentlich (eine aktuelle TExLive Installation vorausgesetzt) funktionieren. 

https://www.lyx.org/Home
<img width="822" height="242" alt="image" src="https://github.com/user-attachments/assets/b969f1b3-8aeb-4bda-b655-b01c69cc5b3c" />


## Grundidee des AVVP‑Themes

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
    <td colspan="2">
      <a href="https://github.com/user-attachments/assets/12af362e-6773-4e8a-9feb-7df1aa42300c">
        <img src="https://github.com/user-attachments/assets/12af362e-6773-4e8a-9feb-7df1aa42300c" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/user-attachments/assets/9df4c369-3747-4960-aa12-ad97c4801909">
        <img src="https://github.com/user-attachments/assets/9df4c369-3747-4960-aa12-ad97c4801909" width="100%" />
      </a>
    </td>
    <td>
      <a href="https://github.com/user-attachments/assets/bb2ce8f2-4f5d-487c-92e0-36a0cf115f45">
        <img src="https://github.com/user-attachments/assets/bb2ce8f2-4f5d-487c-92e0-36a0cf115f45" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/user-attachments/assets/69bb8994-7648-426c-98cc-93f2039a1161">
        <img src="https://github.com/user-attachments/assets/69bb8994-7648-426c-98cc-93f2039a1161" width="100%" />
      </a>
    </td>
    <td>
      <a href="https://github.com/user-attachments/assets/787c11bf-7f71-4809-8046-8780e3c71651">
        <img src="https://github.com/user-attachments/assets/787c11bf-7f71-4809-8046-8780e3c71651" width="100%" />
      </a>
    </td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/user-attachments/assets/e9c395b6-a03c-46bf-8607-126d8ed13dda">
        <img src="https://github.com/user-attachments/assets/e9c395b6-a03c-46bf-8607-126d8ed13dda" width="100%" />
      </a>
    </td>
    <td>
      <a href="https://github.com/user-attachments/assets/9078de16-3764-42e4-971b-153eff7fb8f0">
        <img src="https://github.com/user-attachments/assets/9078de16-3764-42e4-971b-153eff7fb8f0" width="100%" />
      </a>
    </td>
  </tr>
</table>

## BibLaTeX, biber und Bildnachweise

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

## Farbsystem

## Design-Guidelines (CI)

Dieses Theme folgt bewusst einem „CI-light“-Ansatz: CI wird konsequent umgesetzt, wo sie Orientierung und Wiedererkennbarkeit schafft, aber nicht dort, wo Beamer-didaktische Elemente dadurch schlechter lesbar oder weniger intuitiv würden.

Grundprinzipien

- Inhalt vor Gestaltung: Inhalte bleiben Standard-Beamer (frames, itemize, blocks, math, figures). Das Theme steuert primär Farben, Typografie, Header/Footer und wiederkehrende Struktur-Elemente.
- Dark/Light müssen gleichwertig funktionieren: Jede Farbe/Definition wird so gewählt, dass Kontrast und Lesbarkeit in beiden Modi erhalten bleiben.
- Semantik der Farben:
  - AVVPBlue: Struktur/Navigation (Titel, Sections, TOC-Struktur)
  - AVVPSpark: Fokus/Aktiv (alerted text, aktive Marker)
  - AVVPGrey: Dezent/inaktiv (gedimmte Elemente)
- Didaktische Beamer-Elemente bleiben neutral, wenn CI-Farben den didaktischen Zweck schwächen würden (z. B. Block- und Exampleblock-Optik).

## Schriftarten & Typografie

Dieses Theme setzt auf LuaLaTeX/XeLaTeX (fontspec) und nutzt die mitgelieferten OTF/TTF-Schriften aus `fonts/`.

Verwendete Schriften (Standardkonfiguration)

- Exo 2: Hauptschrift für Präsentationstext (Sans)
- Share Tech Mono: Monospace (Code/ERT/Technik)

Wichtige Hinweise

- Engine: PDFLaTeX wird nicht unterstützt (fontspec benötigt LuaLaTeX oder XeLaTeX).
- Lesbarkeit: Hervorhebungen erfolgen primär über Farbe/Marker (AVVPSpark), nicht über Fettdruck.
- Warnung (wichtig fuer LyX): Das Verzeichnis mit den Schriften muss im Projekt-Root exakt `fonts/` heissen (neben `AVVP_VORLAGE_BEAMER.lyx`). Den Verzeichnisnamen nicht aendern und das Verzeichnis nicht verschieben.

Hintergrund: LyX exportiert/kompiliert die `.tex` im Projekt-Root. Das Theme bindet die OTF/TTF-Dateien per `fontspec` mit relativen Pfaden wie `Path = fonts/...` ein. Umbenennen/verschieben fuehrt in LuaLaTeX/luaotfload typischerweise zu Font-Load-Fehlern (z.B. fehlende OTF-Dateien, `cannot find file ''`, \"metric data not found\").

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

## Dark‑ und Light‑Mode

### Aktivierung

Das Theme kennt zwei Modi, die **global** gesetzt werden. In LyX ist das typischerweise in der Dokument‑Präambel (oder in `avvp_beamer_preamble.tex`) zu platzieren.

```latex
\AVVPDarkMode
% oder
\AVVPLightMode
```

Diese Makros setzen **global** u. a.:
- Hintergrund‑ und Textfarben
- Farben für Struktur‑Elemente (Titel/Section/Subsection/TOC)
- Farben für Listen (Bullets/Enumerate)
- Overlay‑Dimmung (Beamer `covered`‑Verhalten)
- Footer/CI‑Farben (abhängig vom Modus)

### Overlay‑Dimmung

Die Dimmung nicht‑aktiver Inhalte wird zentral gesteuert (Beamer `\setbeamercovered`). Typischerweise nutzt das Theme einen festen Transparenzwert, der im Dark‑ und Light‑Mode passend gewählt ist.

Wichtig: Wenn du Frames hast, die **keine** Overlays erzeugen sollen (z. B. reine Bildfolien), setze im Frame eine Overlay‑Spezifikation wie `<*>`:

```latex
\begin{frame}<*>{Titel}
  ...
\end{frame}
```

---

## Bulletpoints & Nummerierungen

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

## Overlays

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

## Header (angepasst)

Der Header ist **nicht mehr** der Beamer‑Standard von *Malmoe*, sondern wurde für das AVVP‑Layout angepasst.

Charakteristik:
- klare Trennung von Abschnitt / Unterabschnitt
- konsistente CI‑Farben (AVVPBlue / AVVPSpark / AVVPGrey)
- aktive Navigationselemente werden visuell hervorgehoben

Wichtig:
- Der Header wird im Theme vollständig kontrolliert (keine Dokument‑Hacks nötig).

### Agenda‑Zeile im Footer (oben) gezielt abschalten

Das Theme rendert oberhalb der eigentlichen Fußzeile optional eine **Agenda‑Zeile** (Section/Subsection‑Navigation). Diese lässt sich global oder lokal deaktivieren.

Global (z. B. in der Dokument‑Präambel):

```latex
\AVVPAgendaBarOff
% ... später wieder aktivieren:
% \AVVPAgendaBarOn
```

Lokal (nur für einen Block / Frame):

```latex
\AVVPWithAgendaBarOff{%
  \begin{frame}{Bildfolie ohne Agenda}
    ...
  \end{frame}
}
```

Hinweis:
- Die lokalen Wrapper wirken nur innerhalb des Blocks und verändern keine globalen Einstellungen.

---

## Zwischenagenda (automatisch)

Das Theme erzeugt automatisch Zwischenfolien (Agenda‑Frames) an definierten Struktur‑Punkten.

### Automatik

Die Zwischenagenda wird über Beamer‑Hooks erzeugt (keine manuellen Agenda‑Frames im Dokument nötig). Das Theme stellt dafür zwei Modi bereit:

1) Fokus nur auf **Sections**

```latex
\AVVPAgendaOnlySections
```

2) Fokus auf **Sections & Subsections** (Default)

```latex
\AVVPAgendaSectionsAndSubsections
```

Wirkung:
- **Sections & Subsections (Default):** Zwischenagenda bei jedem Subsection‑Start.
- **Only Sections:** Zwischenagenda nur bei Section‑Starts.

### Titel der Zwischenagenda

Der Titel der Zwischenagenda‑Folie ist als Macro konfigurierbar:

```latex
\def\AVVPSubAgendaTitle{Weiter geht's im Text ...}
```

### Robustheit / Overlays

Wenn du global Overlays (z. B. `<+->`) nutzt und eine Zwischenagenda‑Folie **immer** in einem Schritt erscheinen soll, kannst du in solchen Frames eine Overlay‑Spezifikation wie `<*>` setzen.

---

## Footer (CI‑gebunden)

Der Footer ist **neu** und eng an das CI gekoppelt. Er ist kein Beamer‑Standard‑Footer.

Charakteristik:
- CI‑konforme Farbflächen (abhängig von Dark/Light)
- definierte Zonen für Autor/Kurztitel/ggf. Datum bzw. Projekthinweise (je nach Konfiguration)
- Logo wird kontrolliert gesetzt (keine automatische Beamer‑Logo‑Mechanik)

Wichtig:
- Das Beamer‑Standard‑Logo (`\logo{...}`) ist deaktiviert bzw. wird im Theme nicht benutzt.
- Das Logo erscheint nur dort, wo das Theme es explizit im Footer platziert.

## Neue Makros (Kurzreferenz)

Die wichtigsten Makros, die im Theme verwendet werden (Auszug):

- `\AVVPDarkMode` / `\AVVPLightMode` – setzt global den Modus inkl. Farben/Overlays.
- `\AVVPAgendaFrame` – erzeugt die Zwischenagenda‑Folie (wird typischerweise automatisch via `\AtBeginSubsection` aufgerufen).
- `\AVVPLogo` – liefert das CI‑Logo für kontrollierte Platzierung durch das Theme.

Hinweis: Die vollständige Liste ergibt sich aus `avvp_beamer_preamble.tex`. Diese README dokumentiert bewusst nur die stabilen, für Anwender relevanten Einstiegspunkte.

---

## Titelfolie (LyX)

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

## Status

✔ LyX‑stabil  
✔ Dark / Light sauber getrennt  
✔ Header / Footer kontrolliert  

**AVVP Beamer Theme – Stand 01-2026**

## Verwendung ohne LyX (reines LaTeX / Beamer)

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

## Lizenz

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
