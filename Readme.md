# AVVP Beamer Theme für Lyx – README

Dieses Dokument beschreibt **Aufbau, Struktur und Wirkungsweise** der AVVP‑Beamer‑Präambel.
Es erklärt **was wo definiert ist** und **welchen Effekt** Abschnitte, Unterabschnitte, Overlays,
Header, Footer und Agenda‑Frames haben – insbesondere im Zusammenspiel mit **LyX**.

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

<img width="1479" height="834" alt="image" src="https://github.com/user-attachments/assets/12af362e-6773-4e8a-9feb-7df1aa42300c" />
<img width="1478" height="834" alt="image" src="https://github.com/user-attachments/assets/9df4c369-3747-4960-aa12-ad97c4801909" />
<img width="1484" height="837" alt="image" src="https://github.com/user-attachments/assets/bb2ce8f2-4f5d-487c-92e0-36a0cf115f45" />
<img width="1478" height="831" alt="image" src="https://github.com/user-attachments/assets/69bb8994-7648-426c-98cc-93f2039a1161" />
<img width="1478" height="830" alt="image" src="https://github.com/user-attachments/assets/787c11bf-7f71-4809-8046-8780e3c71651" />
<img width="1353" height="764" alt="image" src="https://github.com/user-attachments/assets/e9c395b6-a03c-46bf-8607-126d8ed13dda" />
<img width="1351" height="761" alt="image" src="https://github.com/user-attachments/assets/9078de16-3764-42e4-971b-153eff7fb8f0" />


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

### 7.1 Agenda‑Bar im Footer (zweizeilig)

Zusätzlich zur Agenda‑Zwischenfolie besitzt das AVVP‑Theme eine **optionale Agenda‑Leiste oberhalb des Footers**.

Eigenschaften:

- Zeigt **aktuellen Abschnitt** und **aktuellen Unterabschnitt**
- Wird **automatisch ausgeblendet** auf:
  - der Haupt‑Agenda (vollständiges Inhaltsverzeichnis)
- Bleibt **sichtbar** auf:
  - normalen Inhaltsfolien
  - Unter‑Agenda‑Folien (current subsection)
- Keine manuelle Steuerung im Dokument notwendig

Technische Umsetzung:

- Zweizeilige Footline:
  - Zeile 1: Agenda‑Bar
  - Zeile 2: regulärer Malmoe‑Footer
- Erkennung von ToC‑Frames erfolgt automatisch
- LyX‑sicher (keine ERT‑Blöcke im Dokument nötig)

Grundprinzip:

> Die Agenda‑Leiste unterstützt die Orientierung,
> ohne auf Agenda‑Folien redundant zu erscheinen.

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

Die Agenda‑Bar ist **kein Teil des Footers**, sondern eine
zusätzliche Zeile darüber und wird separat gesteuert.

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

---

## 13. Arbeiten mit Grafiken in LyX (empfohlene Praxis)

Für komplexe Layouts (z. B. mehrere Icons oder Workflow‑Grafiken in einer Zeile)
wird **kein freies Positionieren** empfohlen.

Stattdessen:

- 1‑zeilige Tabelle mit mehreren Spalten verwenden
- Tabellenlinien deaktivieren
- Jede Grafik in eine eigene Zelle setzen
- Zellausrichtung:
  - horizontal: zentriert
  - vertikal: mittig

Vorteile:

- Saubere vertikale Ausrichtung aller Grafiken
- Reproduzierbares Layout
- Vollständig LyX‑kompatibel
- Keine ERT‑Hacks nötig

Diese Methode wird im AVVP‑Theme explizit empfohlen.

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
