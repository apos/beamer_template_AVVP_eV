# AVVP Beamer Theme – README

Dieses Dokument beschreibt **Aufbau, Struktur und Wirkungsweise** der AVVP‑Beamer‑Präambel.
Es erklärt **was wo definiert ist** und **welchen Effekt** Abschnitte, Unterabschnitte, Overlays,
Header, Footer und Agenda‑Frames haben – insbesondere im Zusammenspiel mit **LyX**.

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
