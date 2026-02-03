# README – Web-Markdown aus Beamer (AVVP)

## Ziel
Aus einer bestehenden Beamer-Präsentation (Quelle) soll eine **lesbare Web-Seite als Markdown** erzeugt bzw. aktualisiert werden.

**Single Source of Truth:** Die Beamer-Datei ist die inhaltliche Referenz. Das Web-Markdown ist ein abgeleitetes Ausgabeformat.

## Dateien / Rollen
- **Quelle (Inhalt):** `AVVP_HDR_Teil_2_BEAMER.tex` (bzw. die jeweils aktuelle Beamer-Hauptdatei)
- **Ziel (Web-Markdown):** `docs/AVVP_HDR_Teil_2_web.md`
- **Assets (lokal):** `media/`, `pics/`

## Grundprinzipien (Requirements)
1. **Struktur beibehalten:** Überschriften-Hierarchie folgt der Beamer-Struktur:
   - `\section` → `## ...`
   - `\subsection` → `### ...`
   - `\begin{frame}{Titel}` → `#### Titel` (oder passend als kurzer Unterabschnitt)
2. **Fließtext statt Folien-Stichpunkte:** Stichpunkte werden dort, wo es sinnvoll ist, in **ansprechenden Fließtext** umformuliert.
   - Listen bleiben nur dort Listen, wo es inhaltlich wirklich eine Liste ist (Schritte, Aufzählungen, Merkmale).
3. **Keine Beamer-/LaTeX-Artefakte im Text:** In der Webfassung dürfen keine Folien-Steuerzeichen auftauchen (z.\,B. `\pause`, Overlay-Spezifikationen wie `<1->`, Layout-Hacks wie `\vspace`).
4. **Bilder als Markdown, lokal referenziert:**
   - Bilder werden als Markdown eingebunden, z.\,B. `![](../media/datei.png)`.
   - Es werden **nur lokale Projektpfade** verwendet (keine absoluten Pfade vom Entwicklerrechner).
5. **Keine externen Ressourcen einbinden:** Keine Remote-Assets (keine extern gehosteten Bilder/ifames/scripts). Alles liegt im Projekt.
6. **Videos als Poster + Textlink:**
   - Ein Video wird im Web-Markdown als **Posterbild** plus **Textlink** dargestellt.
   - Keine Einbettung/Player erzwingen; der Link ist ausreichend.
7. **Codebeispiele als Codeblöcke:**
   - Code erscheint als fenced code block (z.\,B. ```tex ... ```).
   - Der Text drumherum erklärt kurz, warum/wofür der Code steht.
8. **Ton & Zielgruppe:**
   - Die Webfassung ist „artikelwürdig“: klare Sätze, kurze Absätze, nachvollziehbarer roter Faden.
   - Keine Platzhalter wie „TBD“, „...“ oder reine Template-Sätze; falls in der Quelle vorhanden, im Markdown entweder entfernen oder als klaren Hinweis markieren.

## Workflow (empfohlen)
1. Beamer-Datei als Referenz lesen und grobe Gliederung (section/subsection/frame-Titel) erfassen.
2. Pro Frame den Kerninhalt extrahieren und zu einem kurzen Absatz (oder einer sinnvollen Liste) verdichten.
3. Medien übernehmen:
   - `\includegraphics{...}` → `![](../media/...)`
   - Video-Frames → Posterbild + Textlink
4. Ergebnis prüfen:
   - Überschriften-Hierarchie stimmt
   - Alle Bildpfade existieren im Projekt
   - Keine LaTeX-Kommandos stehen „roh“ im Fließtext

## Definition of Done
- `docs/AVVP_HDR_Teil_2_web.md` ist ohne weitere Verarbeitung als Web-Artikel nutzbar.
- Struktur entspricht der Beamer-Struktur.
- Bilder werden korrekt aus `media/`/`pics/` geladen.
- Text ist verständlich, flüssig und frei von Beamer-/LaTeX-Steuerartefakten.