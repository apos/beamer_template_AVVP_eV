# README – KOMA-Artikel aus Beamer (AVVP Magazin-Stil)

## Ziel
Aus einer bestehenden Beamer-Präsentation (`AVVP_VORLAGE_BEAMER.tex`) soll eine **inhaltlich äquivalente** KOMA-Script-Artikeldatei im **Magazin-Stil** erzeugt/aktualisiert werden (`AVVP_ARTIKEL_KOMA.tex`).

**Single Source of Truth:** Die Beamer-Datei ist die inhaltliche Referenz. Der Artikel ist ein abgeleitetes Ausgabeformat.

## Dateien / Rollen
- **Quelle (Inhalt):** `AVVP_VORLAGE_BEAMER.tex`
- **Ziel (Artikel):** `AVVP_ARTIKEL_KOMA.tex`
- **Design/Regeln (zentral):** `tex/avvp_koma_preamble.tex`

## Grundprinzipien (Requirements)
1. **Inhalt folgt Beamer:** Überschriften-Struktur und Textinhalt im Artikel müssen der Beamer-Version entsprechen.
2. **Design gehört in die Präambel:** Alle Layout-/Design-Entscheidungen werden in `tex/avvp_koma_preamble.tex` getroffen, nicht im Dokument.
3. **Dokument ist „clean content“:** `AVVP_ARTIKEL_KOMA.tex` soll möglichst nur Struktur + Inhalt enthalten.
4. **Makro-Kompatibilität ist Pflicht:** Bei Konvertierung dürfen **keine neuen, artikel-spezifischen Makro-Namen** erfunden werden.
   Stattdessen müssen **die gleichen Makro-Namen/Signaturen** wie in `tex/avvp_beamer_preamble.tex` verwendet werden; die Magazin-Präambel implementiert diese Makros nur anders (Artikel-Layout statt Folien-Layout).
5. **Keine externen Ressourcen:** Keine Remote-Assets einbinden; alles liegt lokal im Projekt (z.B. unter `media/`, `pics/`).

## Was im Artikel stehen darf (und was nicht)
### Erlaubt im Artikel (`AVVP_ARTIKEL_KOMA.tex`)
- Dokument-Metadaten: `\title`, `\subtitle`, `\author`, `\date`
- Struktur: `\section`, `\subsection`, `\subsubsection*` (oder vereinheitlichte Struktur)
- Reiner Inhaltstext
- Aufrufe der **Beamer-kompatiblen** Makros (z.B. `\AVVPVideoURL`, `\AVVPVideoLocal`, `\AVVPCreditInline`)

### Nicht erlaubt / zu vermeiden im Artikel
- Einzelne „Design-Entscheidungen“ pro Stelle, z.B.:
  - `\centering`, feste `width=...`, `\vspace`, manuelle Caption-Formatierung
  - Umgebungs-spezifische Layout-Hacks (außer sie sind inhaltlich zwingend)
- Wiederholte LaTeX-Layouts, die als Makro in die Präambel gehören

## Mapping-Regeln: Beamer → KOMA (Magazin)
Die KI soll den Artikel so erzeugen, dass er **lesbar als Fließtext** ist, aber **inhaltlich deckungsgleich** bleibt.

**Wichtig:** Bei allen Umsetzungen gilt: **Makro-Namen aus der Beamer-Präambel beibehalten**. Wenn ein Beamer-Makro im Artikel gebraucht wird, wird es in `tex/avvp_koma_preamble.tex` entsprechend (magazin-tauglich) bereitgestellt – aber der Aufruf im Artikel bleibt gleich.

### Struktur
- `\section{...}` bleibt `\section{...}`
- `\subsection{...}` bleibt `\subsection{...}`
- Beamer-Frames werden im Artikel in passende Abschnitte/Unterabschnitte überführt:
  - `\begin{frame}{Titel}` → typischerweise `\subsubsection*{Titel}` (oder Absatzüberschrift), **ohne** Frame-Layout.

### Listen
- `itemize`/`enumerate` werden 1:1 übernommen.
- Beamer Overlays (`<1->`, `\pause`) werden im Artikel entfernt.

### Theorem/Blocks
- Beamer-`block`, `exampleblock`, `alertblock` werden im Artikel als ruhige Magazin-Entsprechung umgesetzt:
  - Standard: `\paragraph{...}` + Text/`itemize`
  - (Optional später) Eigene Makros/Umgebungen in der Präambel, falls benötigt.

### Code-Listings
- Beamer `lstlisting`/fragile: im Artikel als normales `verbatim` oder `listings` (wenn gewünscht), aber **Stilentscheidung zentral in der Präambel**.

### Abbildungen
- Abbildungen werden im Artikel als normale `figure` gesetzt (mit `\includegraphics{...}`), aber:
  - **kein automatisches Cropping/Trim** in der Konvertierung.
  - **keine individuellen Layout-Hacks im Dokument** (kein `\vspace`, keine wechselnden Breitenkonzepte).
- Standard-Skalierung soll aus der Magazin-Präambel kommen (Default `Gin`-Keys etc.).
- Bildnachweis:
  - Quellen werden als Cite-Key geführt (BibLaTeX) und im Artikel z.B. als `\AVVPCreditInline{cite-key}` im Caption integriert.

### Videos/Links (Poster + URL)
- Ein „Video“ im Artikel ist eine URL + Posterbild (kein eingebettetes Video).
- Es werden die Beamer-Makros verwendet:
  - `\AVVPVideoURL{<url>}{<poster>}` für Web-Links
  - `\AVVPVideoLocal{<pfad-zur-datei>}{<poster>}` für lokale Dateien
- Die Magazin-Präambel sorgt dafür, dass diese Makros im Artikel sinnvoll aussehen (Skalierung, Linktext unter dem Poster, etc.).

## Bibliographie / Quellen
- Die BibLaTeX-Konfiguration bleibt im Artikel **minimal**, typischerweise:
  - `\usepackage[style=authoryear]{biblatex}`
  - `\addbibresource{bibtex.bib}`
- Ausgabe am Ende:
  - `\printbibliography[title={Quellenverzeichnis}]`
- Zusätzliche Formatierungsregeln (z.B. Anzeige des cite-key) gehören in die Präambel.

## Magazin-Präambel: Verantwortlichkeiten
`tex/avvp_koma_preamble.tex` ist der Ort für:
- Farben, Schriften, CI
- Caption-Format, Float-Verhalten
- Standard-`\includegraphics`-Defaults
- Makros für Abbildungen/Video-Referenzen/Quellenhinweise
- Optional: Tabellen-/Code-Stile als zentrale Makros

## Workflow für die KI (konkrete Anforderungen)
Wenn die KI „Beamer → Artikel“ ausführt, soll sie:
1. **Beide Dateien vergleichen:** `AVVP_VORLAGE_BEAMER.tex` vs. `AVVP_ARTIKEL_KOMA.tex`.
2. **Inhaltliche Differenzen beheben:** Text/Struktur angleichen (Beamer ist Referenz).
3. **Overlays entfernen:** `\pause`, Overlay-Spezifikationen (`<...>`) eliminieren.
4. **Layout zentralisieren:** Wiederholtes Layout im Artikel durch Präambel-Makros ersetzen.
5. **Medien konsistent umsetzen:**
   - Beamer-Video-Links + GIFs → `\AVVPVideoURL{...}{poster}` bzw. `\AVVPVideoLocal{...}{poster}`
   - Bilder → normale `\includegraphics{...}` in `figure` (ohne Trim)
6. **Keine lokalen Pfade aus Entwicklerrechnern:** keine absoluten Pfade, nur Projektpfade.
7. **Ergebnis prüfen:** Artikel kompiliert mit LuaLaTeX und nutzt nur lokale Assets.

## Definition of Done
- `AVVP_ARTIKEL_KOMA.tex` enthält überwiegend Inhalt + Struktur, keine Layout-Hacks.
- Alle wiederkehrenden Layoutmuster sind in `tex/avvp_koma_preamble.tex` kapsuliert.
- Inhaltliche Übereinstimmung mit Beamer (gleiche Botschaft/Abschnitte/Medien).
- Quellen/Bildnachweise funktionieren über BibLaTeX.
- Ausgabe-PDF entsteht beim Kompilieren automatisch als `AVVP_ARTIKEL_KOMA.pdf`.