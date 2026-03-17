# Style Guide: Kursplan (Frontend Anzeige) - Frohnhof OS

Dieser Style Guide beschreibt die visuellen und interaktiven Elemente des Frontend-Moduls "Kursplan", wie es in Version v6 entwickelt wurde. Er dient als Referenz für zukünftige Erweiterungen und stellt sicher, dass das Design konsistent bleibt.

## 1. Typografie

*   **Primäre Schriftart:** `Poppins`
    *   **Einsatz:** Alle Texte (Header, Kurstitel, Beschreibungen, Labels, Footer).
    *   **Fallback:** `-apple-system, BlinkMacSystemFont, sans-serif` (Systemschriften für optimale Darstellung auf verschiedenen Geräten).
*   **Schriftgewichte (wght):**
    *   `300` (Light) - Selten, für sehr subtile Texte.
    *   `400` (Regular) - Standard-Fließtext, Untertitel im Header.
    *   `500` (Medium) - Legend-Items, Buttons.
    *   `600` (SemiBold) - Kurszeiten, Labels, Überschriften in Info-Cards.
    *   `700` (Bold) - Hauptüberschriften (`<h1>`), Kursnamen.
    *   `800` (ExtraBold) - Für besondere Akzente, sparsam verwenden.

## 2. Farbpalette

Die Farbpalette ist auf warme Erdtöne und sanfte Pastellfarben abgestimmt, um ein professionelles und einladendes Erscheinungsbild zu gewährleisten.

### 2.1 Grundfarben
*   `--bg`: `#f8f6f3` (Hintergrundfarbe, warmer Cremeton)
*   `--bg-card`: `#ffffff` (Hintergrundfarbe für Karten/Blöcke, reines Weiß)
*   `--text`: `#2d2a26` (Primäre Textfarbe, dunkles Braun/Anthrazit)
*   `--text-sub`: `#6b6560` (Sekundäre Textfarbe, mittleres Braun/Grau)
*   `--text-muted`: `#9e9893` (Gedämpfte Textfarbe, helles Braun/Grau, für Metadaten)
*   `--border`: `rgba(0,0,0,0.06)` (Leichte Rahmenfarbe für Trennlinien und Kartenränder)

### 2.2 Header-Farbverlauf
*   `linear-gradient(135deg, #2d2a26 0%, #4a4540 40%, #3d3832 100%)` (Dunkles, warmes Braun mit dezentem Verlauf)

### 2.3 Kurs-Kategorien (Pastell-Töne)
Diese Farben werden für die Seitenleiste der Kursblöcke (`::before`), Hintergrund-Glow-Effekte und Modal-Icons verwendet.

*   **Ponyclub:** `#6bba70` (Sanftes Grün)
*   **PC-Maxi:** `#4db6ac` (Sanftes Türkis)
*   **Longe:** `#64b5f6` (Sanftes Blau)
*   **Freireiter:** `#ffb74d` (Sanftes Orange)
*   **FR Team:** `#ba68c8` (Sanftes Violett)
*   **KJP / Therapie:** `#f06292` (Sanftes Rosa)
*   **Beritt:** `#a1887f` (Sanftes Braun)
*   **Springstunde:** `#e57373` (Gedämpftes Rot)
*   **Erwachsene:** `#90a4ae` (Blau-Grau)

### 2.4 Ampel-Funktion
*   **Grün (Frei):** `--green`: `#4caf50`
*   **Gelb (1 Platz frei):** `--yellow`: `#ffc107`
*   **Rot (Ausgebucht):** `--red`: `#ef5350`

## 3. Design-Prinzipien & Komponenten

### 3.1 Header (Top-Bar)
*   **Logo:** Groß (ca. 100px Höhe auf Desktop, 60px auf Mobile), linksbündig. Das `Logo_FH_transparent.png` wird mit `filter: brightness(0) invert(1);` in Weiß dargestellt.
*   **Titel:** `Kursplan [Standort]` direkt neben dem Logo, gefolgt von `Kinderreitschule Köln – Frohnhof e.V.` als Untertitel und dem `Frühjahr 2026 · [Adresse/Standort]` darunter.
*   **Druck-Button:** Rechtsbündig, als dezenter Button im Header integriert (`🖨️ Drucken / PDF`). Wird im Druck ausgeblendet.

### 3.2 Foto-Streifen (Online-Version)
*   **Position:** Direkt unter dem Header.
*   **Layout:** Horizontal scrollbarer Streifen mit `flexbox` und `overflow-x: auto;`. Scrollbalken sind verborgen.
*   **Karten:** `photo-card` mit `width: 180px`, `height: 120px` (Desktop). `object-fit: cover` für Bilder. Enthält ein `photo-label` am unteren Rand mit Farbverlauf (`linear-gradient(transparent, rgba(0,0,0,0.7))`).
*   **Interaktion:** `hover` mit `transform: translateY(-3px) scale(1.02);` und `box-shadow` für einen leichten Lift-Effekt.

### 3.3 Ampel-Legende
*   **Position:** Zwischen Foto-Streifen und Kursplan-Grid.
*   **Inhalt:** Auflistung der Kurs-Kategorien mit farbigen Punkten und eine Erklärung der Ampel-Farben (Grün, Gelb, Rot) mit kleinen Punkten.

### 3.4 Kursplan-Grid
*   **Layout:** CSS Grid-Layout. 6 Spalten auf Desktop (Esch), 3 Spalten (Michaelshoven), 2 Spalten auf Tablet, 1 Spalte auf Mobile.
*   **Tages-Spalten (`day-col`):** Hintergrund `var(--bg-card)`, abgerundete Ecken (`var(--radius)`), leichter `box-shadow` bei `hover`.
*   **Tages-Header:** Kurs `<h3>` (z.B. Montag), darunter `[Anzahl] Kurse` in `text-muted`.

### 3.5 Kurs-Blöcke (`course-block`)
*   **Design:** Relativ positioniert, abgerundete Ecken (`var(--radius-sm)`), farbige linke Border (`3px solid var(--c-kurstyp)`).
*   **Inhalt:** Kurszeit (`course-time`), Kurs-Emoji und Kursname (`course-name`), sowie Belegungsstatus (`course-spots`).
*   **Ampel:** Kleiner farbiger Kreis (`ampel green/yellow/red`) in der rechten oberen Ecke.
*   **Interaktion (Online):**
    *   **Hover:** `transform: translateY(-2px);` und `box-shadow` für einen Lift-Effekt.
    *   **Mobile Touch:** Erster Tap highlightet den Kurs (`.touch-active` Klasse, leichter Filter + Outline). Zweiter Tap öffnet das Modal. (`touchstart`, `click` Events).

### 3.6 Modal (Detailansicht)
*   **Trigger:** Klick/Tap auf einen Kursblock.
*   **Design:** Zentrales Modal-Overlay mit `backdrop-filter: blur(8px)` (Glassmorphism-Effekt). `modal-card` mit abgerundeten Ecken und `box-shadow`.
*   **Inhalt:** Kurs-Emoji, Kursname, Zeit/Tag, Kurzbeschreibung (aus `TYPES`), **Platz-Statistik (gesamt, belegt, frei)** und **Trainer-Name (Platzhalter)**.
*   **Schließen:** Button (`✕`), Klick außerhalb des Modals, oder `Escape`-Taste.

### 3.7 Kursbeschreibungen (unterhalb des Plans)
*   **Layout:** CSS Grid, 4 Spalten auf Desktop (Esch), 3 Spalten (Michaelshoven), 2 Spalten auf Tablet, 1 Spalte auf Mobile.
*   **Karten:** `desc-card` mit `border-left-color` passend zum Kurstyp, enthält `<h5>` mit Emoji und Titel sowie den Beschreibungstext `<p>`.

### 3.8 Kontakt & Footer
*   **Kontakt:** `contact-card` mit zwei Spalten (Links: Allgemeine Info, Rechts: Annika Breuer Kontaktdaten). Mobil: gestapelt.
*   **Footer:** `footer-bar` mit Copyright und allgemeinen Hinweisen.

## 4. Druck-Optimierung (`@media print`)

*   **Papierformat:** `A4 landscape` mit 8mm Rändern.
*   **Farben:** `print-color-adjust: exact` für exakte Farbwiedergabe (Header, Kursblöcke).
*   **Ausgeblendet:** Online-Foto-Streifen (`photo-strip`), Druck-Button, Modal, Animationen.
*   **Header (Druck):** Kleiner, kompakter, ohne interaktive Elemente.
*   **Kursplan (Druck):** `grid-template-columns: repeat(6, 1fr) !important` (Esch), `repeat(3, 1fr) !important` (Michaelshoven) für optimale Ausnutzung des Platzes.
*   **Fotos im Druck:** Drei `print-photo`-Bilder (aktuell feste Auswahl), die mit leichten Rotationen (`transform: rotate`) "reingewürfelt" sind, um den Activiva-Stil zu imitieren. Diese sind standardmäßig ausgeblendet und nur im Druck sichtbar.
*   **Schriftgrößen:** Angepasst für optimale Lesbarkeit auf gedrucktem DIN A4 (kleinere `font-size`-Werte).

## 5. Entwicklungs-Besonderheiten

*   **Vanilla JS:** Keine externen JavaScript-Bibliotheken oder Frameworks, um Abhängigkeiten zu minimieren.
*   **CSS Custom Properties:** Zentrale Definition von Farben und Abständen zur einfachen Wartung und Skalierbarkeit.
*   **GitHub Pages:** Hosting über `Frohnhof/Wetter` für einfache Veröffentlichung und Zugriff.
*   **To-Do (Backend):** Für die Verwaltung von Kursen, Schülern, Trainern, Pferden und Absagen (sowie die dynamische Anzeige von Namen im internen Plan) ist der Aufbau eines PostgreSQL-Backends mit einem Verwaltungsinterface (z.B. Django, Flask, Node.js) dringend empfohlen.
