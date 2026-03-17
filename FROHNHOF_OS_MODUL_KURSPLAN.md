# Modulbeschreibung: Kursplan (Frontend Anzeige)

## 1. Modulname & Version
*   **Modulname:** Kursplan (Frontend Anzeige)
*   **Version:** v6 (Stand: 17. März 2026)
*   **Zweck:** Webbasierte und druckoptimierte Darstellung der Reitschulkurse für die Standorte Esch und Michaelshoven. Diese Version dient als öffentliche Präsentation und als konzeptionelle Basis für eine spätere interne Verwaltungsansicht.

## 2. Entwicklungshistorie & Kernänderungen (Tag 17.03.2026)

Das Modul wurde über mehrere Iterationen entwickelt, um den Anforderungen an Design, Funktionalität und Datenintegration gerecht zu werden:

*   **Ursprüngliche Versionen (v1-v2):** Erste Entwürfe des Kursplans, Fokus auf Grundlayout und Kurszeiten.
*   **Premium-Design (v3):** Einführung eines hochwertigen, modernen Designs mit Poppins-Font, sanften Pastellfarben und dunklem Hero-Header, inspiriert von Fitnessstudio-Kursplänen.
*   **Einbindung von Fotos (v4):** Implementierung eines horizontal scrollbaren Foto-Streifens zur visuellen Aufwertung.
*   **Header-Redesign:** Das Frohnhof-Logo wurde deutlich vergrößert, linksbündig platziert, mit dem Kursplan-Titel rechts daneben. Der dunkle Header-Bereich wurde optisch reduziert.
*   **Ampel-Funktion:** Integration einer visuellen Statusanzeige (Grün/Gelb/Rot) direkt in den Kursblöcken zur schnellen Erkennung der Kursbelegung.
    *   **Grün:** Mehr als 1 Platz frei
    *   **Gelb:** Genau 1 Platz frei
    *   **Rot:** Ausgebucht (0 Plätze frei)
*   **Erweiterte Klick-Details:** Beim Klick auf einen Kurs öffnet sich ein Modal, das detaillierte Informationen wie den Trainer (aktuell Platzhalter), die Gesamtzahl der Plätze, belegte Plätze und freie Plätze anzeigt.
*   **Kursbeschreibungen:** Die detaillierten Kursbeschreibungen wurden unterhalb des Kursplans als separate Sektion platziert, anstatt im Modal, um eine bessere Übersicht zu gewährleisten.
*   **Druck-Layout:** Eine optimierte `@media print`-Version wurde erstellt. Diese blendet die Online-Fotostreifen aus und integriert stattdessen bis zu drei "reingewürfelte" Fotos im Activiva-Stil in die Druckansicht (aktuell noch ohne direkten "Einwurf", sondern als kleiner Streifen für den Druck).
*   **Vereinheitlichung der Standorte:** Die Designs für Esch und Michaelshoven wurden auf ein identisches Farbschema, Layout und Fonts (Poppins) vereinheitlicht. Die Michaelshoven-Version wurde dazu komplett auf Basis der Esch-Vorlage neu aufgebaut.
*   **Mobile-Optimierung:** Das Layout ist vollständig responsiv (optimiert für Desktop, Tablet, Mobile). Die Interaktion auf Mobilgeräten wurde durch eine "Touch-Highlight / Tap-to-Detail"-Funktion verbessert.
*   **Kontaktdaten:** Die Kontaktdaten wurden auf Annika Breuer (Tel: 01573 - 263 89 51, E-Mail: a.breuer@frohnhof-koeln.de) aktualisiert.

## 3. Verwendete Technologien
Das Modul ist als Single-Page-Application pro Standort aufgebaut:
*   **Frontend:** HTML5, CSS3, JavaScript (Vanilla JS, kein externes Framework).
*   **Styling:** Responsive Design mit `@media` Queries, Verwendung von CSS Custom Properties für Farbmanagement.
*   **Fonts:** Google Fonts (Poppins) für ein modernes Schriftbild.
*   **Print:** Spezifische `@media print`-Regeln für optimierten DIN A4 Querformat-Ausdruck.
*   **Bild-Optimierung:** Fotos wurden für die Webnutzung optimiert (Größe, Qualität) mit Pillow (Python-Bibliothek) und auf GitHub Pages hochgeladen.

## 4. Datenprofil (aktueller Frontend-Stand)

Die Kursdaten werden aktuell direkt im JavaScript der HTML-Datei definiert. Dies dient der Demonstration und einfachen Handhabung für die öffentliche Version. Für eine interne Verwaltung ist ein Backend zwingend erforderlich (siehe "Nächste Schritte").

### 4.1 `TYPES` Objekt (Kurs-Typen Definition)
Ein JavaScript-Objekt, das Metadaten für jeden Kurstyp enthält:

```javascript
const TYPES = {
  ponyclub:     { emoji:'🐴', label:'Ponyclub',       color:' #6bba70', bg:'rgba(107,186,112,0.10)', desc:'Spielerischer Einstieg ab 4 Jahren. Putzen, Führen, erstes Sitzen.' },
  pcmaxi:       { emoji:'🏇', label:'PC-Maxi',        color:' #4db6ac', bg:'rgba(77,182,172,0.10)',  desc:'Fortgeschrittene Ponyclub-Kinder. Bodenarbeit, freies Reiten.' },
  longe:        { emoji:'🔄', label:'Longe',           color:' #64b5f6', bg:'rgba(100,181,246,0.10)', desc:'Einzelunterricht an der Longe. Balance, Haltung, Vertrauen.' },
  freireiter:   { emoji:'🌟', label:'Freireiter',      color:' #ffb74d', bg:'rgba(255,183,77,0.10)',  desc:'Eigenständiges Reiten in der Gruppe. Bahnfiguren und Tempi.' },
  frteam:       { emoji:'🏆', label:'FR Team',         color:' #ba68c8', bg:'rgba(186,104,200,0.10)', desc:'Fortgeschrittene Reitergruppe. Dressurlektionen und Teamarbeit.' },
  kjp:          { emoji:'💚', label:'KJP / Therapie',  color:' #f06292', bg:'rgba(240,98,146,0.10)',  desc:'Pferdegestützte therapeutische Förderung mit Fachkräften.' },
  beritt:       { emoji:'🐎', label:'Beritt & Longe',  color:' #a1887f', bg:'rgba(161,136,127,0.10)', desc:'Professionelles Training der Schulpferde.' },
  springstunde: { emoji:'🏅', label:'Springstunde',    color:' #e57373', bg:'rgba(229,115,115,0.10)', desc:'Springgymnastik und Parcourstraining.' },
  erwachsene:   { emoji:'🧑', label:'Erwachsene',      color:' #90a4ae', bg:'rgba(144,164,174,0.10)', desc:'Reitunterricht für Erwachsene.' }
};
```

### 4.2 `SCHEDULE` Array (Kurs-Schedule pro Standort)
Ein JavaScript-Array, das die Kursplanung pro Tag und Slot enthält. Dies muss für jeden Standort individuell gepflegt werden:

```javascript
// Beispiel für Esch (gekürzt)
const SCHEDULE = [
  { day:'Montag', slots:[
    { type:'kjp',    time:'13:15 – 14:45', label:'Förderschule',  max:null, filled:null, trainer:'— (Platzhalter)' },
    { type:'longe',  time:'15:30 – 16:30', max:3, filled:3, trainer:'— (Platzhalter)' },
    // ... weitere Slots ...
  ]},
  // ... weitere Tage ...
];

// Beispiel für Michaelshoven (gekürzt)
const SCHEDULE = [
  { day:'Freitag', slots:[
    { type:'kjp',      time:'14:00 – 15:00', label:'Therapie / Regenwald', max:null, filled:null, trainer:'— (Platzhalter)' },
    { type:'ponyclub', time:'15:45 – 16:45', max:6, filled:6, trainer:'— (Platzhalter)' },
    // ... weitere Slots ...
  ]},
  // ... weitere Tage ...
];
```

**Attribute pro Slot:**
*   `type` (String): Referenz zum `TYPES`-Objekt.
*   `time` (String): Zeitangabe des Kurses.
*   `label` (String, optional): Spezifische Bezeichnung des Kurses (falls abweichend vom `TYPES.label`).
*   `max` (Number / `null`): Maximale Teilnehmerplätze. `null` bei unbegrenzt/nicht zutreffend.
*   `filled` (Number / `null`): Aktuell belegte Plätze. `null` bei unbegrenzt/nicht zutreffend.
*   `trainer` (String): Name des Trainers. Aktuell Platzhalter.
*   `kids` (Array of Strings): Geplante Kinder pro Slot. **Aktuell noch nicht implementiert, da Datenquelle (Excel) noch nicht vollständig geparst.**

## 5. Dateien & Veröffentlichung

Die Dateien befinden sich lokal im Arbeitsbereich `/home/simon/.openclaw/workspace/` und werden auf GitHub Pages im Repository `Frohnhof/Wetter` veröffentlicht.

*   **HTML-Dateien:**
    *   `kursplan-esch-v3.html` (publiziert als `kursplan-esch.html`)
    *   `kursplan-michaelshoven-v3.html` (publiziert als `kursplan-michaelshoven.html`)
*   **Logo:**
    *   `Logo_FH_transparent.png`
*   **Fotos (für Online-Galerie und Print):**
    *   `photo_dressur.jpg`
    *   `photo_team.jpg`
    *   `photo_turnier.jpg`
    *   `photo_unterricht.jpg`
    *   `photo_springen.jpg`
    *   `photo_pflege.jpg`
    *   `photo_therapie.jpg`
    *   `photo_alltag.jpg`
*   **Veröffentlichungsort:** GitHub Pages, URLs:
    *   https://frohnhof.github.io/Wetter/kursplan-esch.html
    *   https://frohnhof.github.io/Wetter/kursplan-michaelshoven.html

## 6. Nächste Schritte & Offene Punkte für Frohnhof OS

Die aktuelle Frontend-Implementierung ist eine sehr gute Basis. Für ein vollwertiges Modul im "Frohnhof OS" sind jedoch folgende Schritte erforderlich, die ein **Backend** und eine **Datenbank** umfassen:

1.  **Backend-Entwicklung (PostgreSQL):**
    *   **Priorität 1:** Aufbau einer robusten relationalen Datenbank (PostgreSQL empfohlen) zur zentralen Speicherung aller Daten: Kurse, Zeiten, maximale/belegte Plätze, Schülerdaten, Trainerdaten, Pferdedaten, Absagen und Zuordnungen.
    *   **Datenbank-Migration:** Entwicklung von Skripten zum sauberen Import der vorhandenen Daten aus den Excel-Dateien in die neue Datenbank.
2.  **Excel-Parsing (Python):**
    *   Weiterentwicklung des Excel-Parsing-Skripts, um die komplexen Strukturen der Excel-Dateien (`Stundenplan aktuell und Archiv Esch.xlsx`, `Warteliste Frohnhof und Michaelshoven.xlsx` etc.) vollständig zu verstehen und alle relevanten Informationen (insbesondere Trainer- und Kindernamen pro Slot, Absagen) zu extrahieren. Dies ist die Grundlage für die initiale Befüllung der Datenbank.
3.  **Internes Verwaltungsinterface (Backend):**
    *   Entwicklung eines einfachen, passwortgeschützten Web-Interfaces oder einer API, über die Annika und andere autorisierte Personen die Kurspläne verwalten können:
        *   Kurse erstellen/bearbeiten/löschen.
        *   Schüler, Trainer und Pferde verwalten.
        *   **Pferdeeinteilung:** Zuweisung von Pferden zu Kursen und Schülern.
        *   **Absagen:** Eintragen von Absagen, die sich auf die Belegung auswirken.
4.  **Integration des Internen Kursplans (Frontend):**
    *   Erstellung einer separaten, **nicht öffentlich zugänglichen** Version des Kursplan-Frontends (z.B. `kursplan-intern.html`), die dynamisch Daten aus dem neuen Backend lädt.
    *   Diese interne Version wird dann die gewünschten Details anzeigen: Trainer, eine Liste der Kinder (ggf. durchgestrichen bei Absagen) und die genaue Ampelfunktion, die sich in Echtzeit aktualisiert.
    *   Die Pferdeeinteilung wird ebenfalls hier sichtbar sein.
5.  **Authentifizierung & Berechtigungen:** Implementierung eines Systems, das sicherstellt, dass nur autorisierte Benutzer auf die Verwaltungsfunktionen und die interne Kursplan-Ansicht zugreifen können.
