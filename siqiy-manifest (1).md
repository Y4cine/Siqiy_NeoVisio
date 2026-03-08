# Siqiy – Vision, Konzept & Feature-Roadmap

> *„Ich kann direkt anfangen zu zeichnen und explorativ und iterativ Automatismen einbauen."*

**Arbeitsdokument · März 2026**

---

## 1. Warum dieses Projekt?

Visio ist das einzige Werkzeug das intelligente, skriptierbare Diagramme ermöglicht – und es stirbt. Falsche Prioritäten bei Microsoft, fehlende Innovation, und der Aufstieg von KI-Werkzeugen lassen eine kritische Lücke entstehen.

Die bestehenden Alternativen sind entweder Zeichenprogramme ohne Intelligenz (Inkscape, draw.io), oder rein codebasierte Lösungen die einen Programmierer voraussetzen (D3.js, Mermaid). Das Mittelfeld ist leer.

Siqiy besetzt genau diese Lücke: ein offenes, modernes Werkzeug das sofort nutzbar ist – und schrittweise so intelligent wird wie man es braucht.

---

## 2. Der Name: Siqiy

**Siqiy** (ausgesprochen: *see-key*) ist das Quechua-Wort für „eine Linie ziehen" bzw. „zeichnen".

### Warum Siqiy?

Der Name wurde aus über 30 Kandidaten in mehreren Sprachen ausgewählt und gegen IT-nahe Produkte geprüft. Die Entscheidungskriterien waren:

- **Klang** – kurz, einprägsam, international aussprechbar
- **Unverwechselbarkeit** – keine Kollision mit existierenden IT-Produkten, Domains oder Paketen (npm, PyPI, GitHub)
- **Bedeutung** – direkte etymologische Verbindung zum Zeichnen
- **Verfügbarkeit** – siqiy.io und verwandte Domains sind unregistriert

Eliminierte Kandidaten: VELA (Constellation Software), Diagen (Uni München / Drug Discovery), Kartos (Cybersecurity), Kroki (Diagram-API), Skema (BIM-Tool), Grafio (iOS-App), Tecto (mehrfach belegt), Vizra (KI-Plattform), SODA (SODA PDF), Gravo (Gravotech), und viele weitere.

Siqiy steht für sich selbst – ohne Verweis auf Visio, ohne Imitation. Die nächste Generation von Nutzern muss Visio nicht kennen, um Siqiy zu verstehen.

---

## 3. Kern-Philosophie: Progressive Automation

Das entscheidende Prinzip heißt **Progressive Automation** – kein anderes Tool bietet das heute:

1. Ich zeichne eine Shape – sie funktioniert sofort, ohne Konfiguration
2. Ich bemerke: diese Shape soll sich verhalten – ich öffne das ShapeSheet
3. Ich füge eine Formel hinzu – sie wirkt live, sofort
4. Ich bemerke: mehrere Shapes brauchen das – ich mache daraus einen Master
5. Ich bemerke: das braucht Logik – ich füge ein Script hinzu

Keine Unterbrechung des Flows. Keine Modusumschaltung in eine Programmierumgebung. Der Nutzer entdeckt während er baut – genau wie in Excel.

Niemand der Siqiy entdeckt muss Visio vorher gekannt haben. Die Zielgruppe ist die nächste Generation von Ingenieuren, Technikern und Gestaltern – die komplexe, intelligente Diagramme brauchen, ohne zu programmieren.

---

## 4. Positionierung

| | Intelligenz | Zugänglichkeit |
|---|---|---|
| Zeichenprogramme (Inkscape, Illustrator) | ✗ | ✓ |
| Code-first Tools (D3.js, Mermaid) | ✓ | ✗ |
| **Siqiy** | ✓ | ✓ |

Siqiy ist keine Konkurrenz zu Illustrator, Blender oder CAD. Es ist die Plattform für technisches und strukturiertes Diagramming – P&IDs, Schaltpläne, Fertigungszeichnungen, Infografiken, Geschäftsgrafiken.

### Warum bisherige OSS-Alternativen gescheitert sind

**draw.io** ist technisch stark, aber hat die falsche Philosophie – es ist ein Zeichenprogramm, kein Framework. Shapes sind dumm. Keine Formel-Engine, kein Typ-System, kein echtes Scripting.

**LibreOffice Draw** – zu sehr an Desktop-Paradigmen gebunden, keine Web-DNA, keine Community-Dynamik.

Das eigentliche Scheitern-Muster: Sie bauen das *Aussehen* nach, nicht die *Intelligenz*. Niemand hat das ShapeSheet-Konzept ernsthaft reimplementiert. Das ist der Kern von Siqiy.

---

## 5. Das Herzstück: ShapeSheet und Formel-Engine

Das ShapeSheet-Konzept aus Visio wird übernommen und modernisiert. Jede Shape ist ein lebendes Objekt mit einer Tabelle von Formeln – wie Excel-Zellen die sich gegenseitig referenzieren:

```
Width  = Height * 1.5
FillColor = IF(Prop.Status = "active", RGB(0,255,0), RGB(255,0,0))
LocPinX = Width * 0.5
```

Das bedeutet: Shapes haben Verhalten, nicht nur Aussehen. Änderungen propagieren automatisch durch einen reaktiven Dependency-Graph – wie ein räumliches Excel.

### Verbesserungen gegenüber Visio

- Erweitertes Formel-Set – bekannte Lücken aus Visio werden geschlossen
- Visueller Formel-Debugger – heute ein schwarzes Loch in Visio
- Verlinkbare Properties – Text und Eigenschaften zwischen Shapes direkt referenzieren
- Formeln die physische Einheiten verstehen – Druck in bar vs. PSI, automatische Konvertierung

---

## 6. Master-Shapes, Schablonen und dynamische Referenzen

Das Visio-Modell von Zeichnungen, Vorlagen, Schablonen und Skripten wird übernommen – aber um ein entscheidendes Feature erweitert: **dynamische Referenzen**.

### Das Problem in Visio

Wenn ein Master aus einer Schablone in eine Zeichnung eingefügt wird, entsteht eine lokale Kopie – die Verbindung zur Quelle geht verloren. Ändert sich das Ur-Shape, aktualisiert sich nichts.

### Die Lösung: Referenz vs. Kopie

- **Referenz-Instanz** – live mit dem Ur-Shape verbunden, aktualisiert sich automatisch
- **Lokale Kopie** – entkoppelt, eigenständig, aber nachträglich wieder bindbar
- **Component Overrides** – lokale Anpassungen bleiben erhalten, strukturelle Updates vom Ur-Shape werden trotzdem übernommen

Jede Instanz trägt einen expliziten Binding-Status:

| Status | Bedeutung |
|---|---|
| 🔗 Referenz | Synchronisiert mit Ur-Shape |
| ✂️ Lokale Kopie | Entkoppelt, eigenständig |
| ⚠️ Veraltet | Ur-Shape hat sich geändert, Update ausstehend |
| 🔀 Konflikt | Lokale Änderungen + Ur-Shape geändert – Drei-Wege-Merge |

Firmenweite Stencils können sich ändern und alle P&IDs weltweit aktualisieren sich automatisch.

---

## 7. Typ-System für Shapes

Visio hat kein formales Typ-System. Ein Master-Shape definiert Aussehen und Formeln – aber keine Klasse. Vererbung, Interfaces, Polymorphismus fehlen komplett.

Siqiy führt ein Typ-System ein: Shape-Typen mit Vererbung, gemeinsamen Properties und definiertem Verhalten. Ein P&ID-Absperrventil ist eine Unterklasse von Ventil, das seinerseits ein Rohrkomponent ist.

Das ermöglicht Validierung gegen Normen, domänenspezifische Abfragen, und konsistente Frameworks über Zeichnungen hinweg.

---

## 8. Scripting und Automatisierung

VBA stirbt mit Visio. Das neue Scripting-Modell ist modern, offen, und mehrsprachig:

| Sprache | Rolle |
|---|---|
| **TypeScript** | Primärsprache – das Tool selbst ist in TS gebaut, Framework-Builder arbeiten direkt darin |
| **Python** | Erste Klasse via Pyodide (WebAssembly) – Ingenieure denken in Python, alle Bibliotheken verfügbar |

### Event-System

Das Event-System muss mindestens so reichhaltig sein wie in Visio:

- `Shape_Added`, `Shape_Deleted`, `Shape_Changed`
- `Cell_Changed` – reagiert auf Formeländerungen
- `Connection_Added`, `Connection_Deleted`
- `Document_Saved`, `Page_Changed`
- Custom Events durch Frameworks definierbar

---

## 9. Zwei-Ebenen-Architektur

| Ebene | Nutzer | Aufgabe |
|---|---|---|
| **Framework-Builder** | Experten (Ingenieure, Techniker, Domänenspezialisten) | Shape-Typen, Regeln, Formeln und Verhalten definieren |
| **Endnutzer** | Zeichner, Planer, Gestalter | Fertiges Framework benutzen – ohne die Komplexität dahinter zu sehen |

Der Framework-Builder baut das P&ID-Werkzeug. Der Schreiner benutzt es. Beide müssen nicht wissen was der andere tut.

---

## 10. Moderne Features – was Visio nie hatte

### KI und MCP-Integration

- Shapes und ihre Properties sind für KI lesbar und schreibbar
- MCP-Server: Claude, GPT und andere KIs können direkt mit dem Dokument interagieren
- Semantisches Verständnis: die KI versteht das Diagramm, nicht nur das Bild
- Beispiel: *„Füge ein Ventil DN50 zwischen Pumpe 3 und Tank 2 ein"*

### Git-Kompatibilität

- Textbasiertes, offenes Format (JSON/XML) das sinnvolle Diffs erzeugt
- Merge-Konflikte zwischen Zeichnungen auflösbar
- Versionierung von Frameworks und Stencils

### Kollaboration

- CRDT-basiertes Echtzeit-Editing (Yjs oder Automerge)
- Offline-first, Synchronisierung wenn verfügbar
- Konfliktfreie parallele Bearbeitung

### Zeitdimension

- Shape-Zustände: Ventil offen / geschlossen / gestört
- Animation zwischen Zuständen
- Simulation – ein P&ID der durchgespielt werden kann
- Historische Ansichten: wie sah die Anlage 2019 aus?

### Massstab und Einheiten

- Echter physischer Massstab – kritisch für Schreiner, P&IDs, Grundrisse
- Shapes kennen physische Einheiten – Länge, Druck, Temperatur
- Automatische Konvertierung zwischen Einheitensystemen

### Drill-Down und Hierarchie

- Eine Shape kann selbst ein vollständiges Diagramm enthalten
- Klick auf „Pumpengruppe" öffnet die Detailzeichnung
- Bidirektional verlinkt – wie in professionellen SCADA-Systemen

### Topologie und Netzwerk-Modell

- Connectors verstehen was sie verbinden – nicht nur zeichnerisch
- Topologie-Abfragen: *„Zeige alle Shapes die mit Tank 3 verbunden sind"*
- Connectors mit eigenen Properties und Formeln

### Smart Connectors

- Orthogonales Routing – Linien weichen Shapes automatisch aus
- Feste Ankerpunkte (Ports) mit eigenem ShapeSheet-Verhalten
- Rerouting bei Shape-Bewegung: live und performant
- Algorithmus-Basis: A* auf Gitter, erweiterbar auf Visibility Graph

### Validation Engine

- Frameworks definieren Regeln, das Tool prüft sie automatisch
- Norm-Validierung: ein P&ID gegen ISO 10628 prüfen
- Unit-Tests für Shapes – testbar wie Code

### Livedaten-Anbindung

- Shapes zeigen live Sensorwerte aus APIs oder Datenbanken
- Bidirektional: Änderungen im Diagramm schreiben zurück in die Datenquelle
- OPC-UA für Industrieanlagen

---

## 11. Plugin-System

Das Plugin-System ist der multiplikative Faktor – keine Funktion, sondern eine Plattform-Entscheidung. Orientiert an VS Code und Obsidian:

- Einfacher Einstieg – ein Plugin in wenigen Stunden schreibbar
- Reichhaltige API – genug Zugriff um wirklich Nützliches zu bauen
- Marketplace – Entdeckbarkeit, Bewertungen, Ein-Klick-Installation

Mögliche Plugin-Kategorien:

- Neue Shape-Bibliotheken (P&ID, UML, Netzwerk, Elektro ...)
- Export-Formate (DXF, IFC, STEP, SVG, PDF ...)
- Datenanbindungen (SQL, REST, GraphQL, OPC-UA, Excel ...)
- KI-Assistenten spezialisiert auf eine Domäne
- Norm-Validatoren (ISO, DIN, ANSI ...)
- Themes und UI-Erweiterungen

---

## 12. Integration in bestehende Ökosysteme

### Drei Ebenen

| Ebene | Beschreibung |
|---|---|
| **Export** | SVG, PDF, PNG, DXF – selbstverständlich, aber verlustbehaftet |
| **Einbettung** | Office Add-in, Confluence, Notion, Obsidian, VS Code – das Diagramm bleibt lebendig |
| **Format als Standard** | Offenes Format das andere implementieren – wie Markdown |

### Browser-Renderer

Eine Web Component ermöglicht das direkte Einbetten in beliebige Webseiten – das Diagramm wird zum lebendigen Element in der Dokumentation.

### Das Diagramm als Dokumentation

- Nicht Export nach Word – das Diagramm *ist* die Dokumentation
- Shapes tragen Beschreibungen, Normen, Warnhinweise
- Publishable als interaktive HTML-Seite
- Stücklisten und Reports direkt aus Shape-Properties generieren

---

## 13. Technischer Stack

Der Stack wurde in zwei Runden durch sieben unabhängige LLM-Analysen kritisch geprüft und konsolidiert. Die ursprünglichen Vorschläge für Hybrid-Rendering (PixiJS + SVG) und HyperFormula wurden von allen Analysen übereinstimmend abgelehnt und ersetzt.

### Konsens-Ergebnis der externen Reviews

| Komponente | Bewertung | Ergebnis |
|---|---|---|
| PixiJS + SVG Hybrid | ❌ Einstimmig abgelehnt | Koordinaten-Drift, doppelte Hit-Testing, Wartungsalbtraum |
| HyperFormula | ❌ Einstimmig abgelehnt | GPLv3-Lizenz blockiert OSS, Grid-Semantik passt nicht zu Shape-Graph |
| Yjs | ✅ Klarer Konsens | Industriestandard, beste Tooling-Unterstützung |
| React | ✅ Klarer Konsens | Community-Argument schlägt technische Eleganz |
| Tauri | ✅ Klarer Konsens | Web-first, kleiner Footprint, kein Electron-Overhead |

### Rendering-Strategie: Phasenweise

Der Renderer wird hinter einer sauberen Abstraktionsschicht gekapselt – das macht ihn austauschbar ohne Rewrite der Logik:

**Phase 1–2: Konva.js (Canvas 2D)**
- Einheitlicher Render-Pfad, kein Hybrid-Chaos
- Nativ scharfer Text ohne Workarounds
- Integriertes Objektmodell passt zum ShapeSheet-Konzept
- Skaliert bis ~5000 Shapes – ausreichend für fast alle Diagramme

**Phase 3+: PixiJS (pure WebGL, wenn Profiling es erfordert)**
- Nur wenn Performance-Messungen es rechtfertigen
- Ausschliesslich WebGL – kein SVG-Overlay
- Text über SDF-Fonts gelöst

### Architektur-Schichten

```
┌─────────────────────────────────────────────┐
│  UI-Layer                                   │
│  React + TypeScript + shadcn/ui             │
├─────────────────────────────────────────────┤
│  Renderer-Interface (abstrakt)              │
│  Konva.js Phase 1 → PixiJS Phase 3          │
├─────────────────────────────────────────────┤
│  Reaktiver DAG (Formel-Engine)              │
│  Custom Parser + Preact Signals             │
│  Formel-Auswertung immer lokal              │
├─────────────────────────────────────────────┤
│  CRDT-Layer (Yjs)                           │
│  Speichert: Formeln, Werte, Struktur        │
│  Berechnet nicht – nur Daten                │
├─────────────────────────────────────────────┤
│  Routing-Engine                             │
│  Phase 1: Direkte Linien (manuell)          │
│  Phase 2: Custom A* + ELK.js               │
├─────────────────────────────────────────────┤
│  Scripting                                  │
│  TypeScript nativ + Pyodide (Plugin)        │
├─────────────────────────────────────────────┤
│  Desktop                                    │
│  Tauri (Rust) – web-first, desktop-ready    │
└─────────────────────────────────────────────┘
```

### Das kritische Architekturprinzip: Formeln vs. CRDT

Dies ist das grösste technische Risiko des Projekts – alle sieben Reviews nennen es explizit:

> **Formeln werden lokal ausgewertet, niemals im CRDT gespeichert.**

Das bedeutet konkret:
- **Yjs speichert:** Formel-Strings, rohe Property-Werte, Dokumentstruktur
- **Lokale Engine berechnet:** abgeleitete Werte aus Formeln
- **Bei Konflikt** (Nutzer A ändert Formel, Nutzer B löscht referenzierte Shape): "Broken Reference"-Marker statt Absturz – Formel bleibt erhalten, zeigt Fehler an

### Strategie

**Web-first, Desktop-ready.** Lokale Datenhaltung als Default, keine Cloud-Abhängigkeiten im Core. Kollaboration ist in Phase 1 architektonisch vorbereitet aber optional – das ShapeSheet-Modell hat Priorität.

---

## 14. Community-Strategie

Die stärksten Open-Source-Projekte entstehen wenn die Community Ownership fühlt. Blenders Wende kam als die Governance transparent wurde. VS Code gewann durch das Plugin-Ökosystem.

### Was diese Community anzieht

- Offenes, dokumentiertes Format – jeder kann Tools drumherum bauen
- Plugin-System mit einfachem Einstieg
- Showcase-Galerie – fertige Frameworks die zeigen was möglich ist
- Gute Dokumentation von Anfang an
- Discord / Forum – die bestehende visguy.com-Community kann direkt migrieren

### Der stärkste Magnet

Ein bekannter Visio-Experte der öffentlich sein erstes Framework in Siqiy baut. Das ist authentischer als jedes Marketing. Das Wissen das über Jahre im visguy.com-Forum aufgebaut wurde, wird zugänglicher – nicht vergraben.

---

## 15. Inspirationen aus anderen Projekten

| Projekt | Relevante Parallele |
|---|---|
| **Excel** | Kein Tutorial nötig – sofort loslegen, Tiefe entdecken. Niedriger Einstieg, unbegrenzte Tiefe |
| **HyperCard (1987)** | Stärkste direkte Parallele. Karten mit Properties und Scripts. „Der Nutzer ist auch der Programmierer." Zu früh, falsch vermarktet – die Idee war richtig |
| **Minecraft** | Kein Ziel, kein Tutorial. Redstone als progressiv entdeckbare Automatisierung |
| **Grasshopper** | Parametrisches Zeichnen für Architekten ohne Programmierkenntnisse. Formeln kontrollieren Geometrie |
| **Smalltalk** | Alles ist ein Objekt das man live inspizieren und ändern kann. Das ShapeSheet ist ein Smalltalk-Inspector für Shapes |
| **Figma** | Component Overrides – lokale Änderungen + globale Updates. Genau das dynamische Referenz-Modell |
| **VS Code / Obsidian** | Plugin-Ökosystem als Plattform-Multiplikator |

---

## 16. Grobe Roadmap

| Phase | Inhalt |
|---|---|
| **Phase 1 – MVP** | Shape mit Formel-Engine (Dependency-Graph, reaktive Cells), Master/Stencil-Konzept, Dateien speichern und laden, grundsätzliche Scripting-API |
| **Phase 2 – Kern** | Dynamische Referenzen (Referenz vs. Kopie), Typ-System für Shapes, Plugin-System (API + Marketplace), Python-Scripting via Pyodide |
| **Phase 3 – Plattform** | Git-kompatibles Format, KI/MCP-Integration, Kollaboration (CRDT), Browser-Renderer / Office Add-in |
| **Phase 4 – Ökosystem** | Validation Engine, Livedaten-Anbindung, Drill-Down / Hierarchie, Zeitdimension / Simulation |

---

## 17. Formel-Syntax

Die Referenz-Syntax für ShapeSheet-Formeln ist eine der wenigen wirklich unwiderruflichen Entscheidungen – sie beeinflusst Datenmodell, Serialisierung, Git-Diffs, Plugin-API und KI-Lesbarkeit.

### Designprinzipien

- **Explizit über implizit** – `Self.Width` statt nur `Width`
- **JavaScript-nah** – sofort verständlich für jeden der JS oder Python kennt
- **KI-lesbar** – selbsterklärend ohne Kontext
- **Name als Alias, ID als Anker** – der Nutzer schreibt Namen, das System arbeitet mit IDs; Umbenennungen propagieren automatisch wie ein IDE-Refactoring

### Konvention: Gross-/Kleinschreibung

```
Self.Width          # System-Cell – Grossbuchstabe = von Siqiy definiert
Self.Height         # System-Cell
Self.Prop.druck     # Custom Property – Kleinbuchstabe = vom Framework-Builder definiert
Self.Prop.material  # Custom Property
Page.Width          # Dokument-Ebene – Grossbuchstabe
```

### Vollständige Syntax-Referenz

```
# Geometrie (System-Cells)
Self.Width    = Self.Height * 1.5
Self.LocPinX  = Self.Width * 0.5
Self.LocPinY  = Self.Height * 0.5
Self.Angle    = 0

# Custom Properties
Self.Prop.druck      = 10
Self.Prop.farbe      = Self.Prop.druck > 8 ? "#CC0000" : "#00AA00"

# Andere Shapes referenzieren (Name → intern ID)
Self.Width           = Shape("Tank1").Prop.volumen / 100
Self.Prop.foerdermenge = Shape("Pumpe3").Prop.nennleistung * 0.85

# Connectors referenzieren
Self.Prop.fluss      = Connector("Leitung1").Prop.durchfluss

# Seite und Dokument
Self.Width           = Page.Width * 0.1
Self.Prop.einheit    = Doc.Prop.einheitssystem

# Ports
Port("Eingang").X    = 0
Port("Eingang").Y    = Self.Height / 2
Port("Ausgang").X    = Self.Width
Port("Ausgang").Y    = Self.Height / 2

# Funktionen (lowercase, vertraut)
Self.Width           = max(Self.Prop.minbreite, Self.Height * 1.5)
Self.Prop.label      = concat(Self.Prop.typ, "-", Self.Prop.nummer)
Self.FillColor       = rgb(255, 0, 0)

# Einheiten (explizit, automatisch konvertierbar)
Self.Prop.druck      = 10[bar]
Self.Prop.druck_psi  = Self.Prop.druck[psi]    # automatische Konvertierung

# Sperren (Visios GUARD – modernisiert)
Self.Width           = locked(100)              # nicht überschreibbar durch Nutzer

# Fehlerbehandlung
Self.Prop.wert       = Shape("Tank1").Prop.volumen ?? 0   # Fallback wenn Referenz fehlt
```

### Shape-Referenzen: intern

Der Nutzer schreibt immer den Namen. Siqiy speichert intern die ID:

```
# Nutzer schreibt:
Self.Width = Shape("Pumpe3").Prop.nennleistung * 0.85

# Siqiy speichert intern:
Self.Width = Shape(#a3f9).Prop.nennleistung * 0.85
```

Wird `Pumpe3` umbenannt, aktualisiert Siqiy alle Formeln automatisch. Der Nutzer sieht immer Namen, das System arbeitet immer mit stabilen IDs.

---

## 18. Datenmodell

Das Datenmodell ist die Grundlage für alles – Serialisierung, Git-Diffs, CRDT, Plugin-API, KI-Lesbarkeit. Es ist eine der unwiderruflichen Architekturentscheidungen.

### Shape-Objekt

```json
{
  "id": "a3f9",
  "name": "Pumpe3",
  "type": "ref:stencil/pid/pumpen/kreiselpumpe",
  "master": {
    "ref": "stencil://pid-standard/kreiselpumpe",
    "binding": "reference",
    "overrides": ["cells.FillColor", "props.bezeichnung"]
  },
  "cells": {
    "Width":     { "formula": "Self.Height * 1.5",                            "value": 60.0 },
    "Height":    { "formula": null,                                            "value": 40.0 },
    "PinX":      { "formula": null,                                            "value": 120.0 },
    "PinY":      { "formula": null,                                            "value": 80.0 },
    "Angle":     { "formula": null,                                            "value": 0.0 },
    "LocPinX":   { "formula": "Self.Width * 0.5",                             "value": 30.0 },
    "LocPinY":   { "formula": "Self.Height * 0.5",                            "value": 20.0 },
    "FillColor": { "formula": "Self.Prop.status == 'on' ? '#00AA00' : '#CC0000'", "value": "#00AA00" },
    "LineColor": { "formula": null,                                            "value": "#333333" },
    "Visible":   { "formula": null,                                            "value": true },
    "Locked":    { "formula": null,                                            "value": false }
  },
  "props": {
    "bezeichnung": {
      "value": "P-101", "formula": null,
      "type": "string", "label": "Bezeichnung", "unit": null
    },
    "status": {
      "value": "on", "formula": null,
      "type": "enum", "options": ["on", "off", "fault"],
      "label": "Betriebszustand", "unit": null
    },
    "druck": {
      "value": 8.5, "formula": null,
      "type": "number", "label": "Betriebsdruck", "unit": "bar"
    },
    "foerdermenge": {
      "value": 120.0, "formula": "Shape(#b1e5).Prop.volumen / 10",
      "type": "number", "label": "Fördermenge", "unit": "m3/h"
    }
  },
  "ports": {
    "eingang": {
      "x": { "formula": "0",               "value": 0.0 },
      "y": { "formula": "Self.Height / 2", "value": 20.0 },
      "direction": "left",
      "accepts": ["fluid-line"]
    },
    "ausgang": {
      "x": { "formula": "Self.Width",      "value": 60.0 },
      "y": { "formula": "Self.Height / 2", "value": 20.0 },
      "direction": "right",
      "accepts": ["fluid-line"]
    }
  },
  "children": [],
  "meta": {
    "created": "2026-03-08T10:00:00Z",
    "modified": "2026-03-08T14:23:00Z",
    "layer": "equipment",
    "tags": ["pumpe", "kreiselpumpe"],
    "notes": "Hauptversorgungspumpe Kühlkreis"
  }
}
```

### Drei Designprinzipien

**Formula + Value immer zusammen.** Jede Cell trägt sowohl die Formel als auch den zuletzt berechneten Wert. Rendering liest `value`, der reaktive DAG aktualisiert `value` wenn Abhängigkeiten sich ändern. Git-Diffs zeigen ob sich Formel oder nur Wert geändert hat. KI kann das Dokument lesen ohne die Formel-Engine zu kennen.

**Props sind typisiert.** Jede Custom Property kennt Typ, Einheit und Label – das ermöglicht automatisch generierte Property-Panels, Einheitenkonvertierung, Validierung und KI-Verständnis ohne zusätzliche Schema-Dateien.

**Master-Binding im Shape selbst.** Das `master`-Objekt hält Herkunft, Binding-Typ und lokale Overrides fest. Bei einem Stencil-Update entscheidet Siqiy präzise: was propagiere ich, was lasse ich in Ruhe.

### Connector-Objekt

Connectors sind vollwertige Shapes mit zwei Sonderfeldern – kein Bürger zweiter Klasse wie in Visio:

```json
{
  "id": "c7d2",
  "name": "Leitung1",
  "type": "ref:stencil/pid/leitungen/fluessigkeit",
  "connection": {
    "from": { "shapeId": "a3f9", "port": "ausgang" },
    "to":   { "shapeId": "b1e5", "port": "eingang" }
  },
  "cells": {
    "LineColor":  { "formula": null, "value": "#0055AA" },
    "LineWeight": { "formula": null, "value": 2.0 },
    "Routing":    { "formula": null, "value": "orthogonal" }
  },
  "props": {
    "medium":     { "value": "Wasser", "type": "string", "label": "Medium",     "unit": null },
    "durchfluss": { "value": 120.0,    "type": "number", "label": "Durchfluss", "unit": "m3/h" }
  }
}
```

### Dokument-Struktur

```json
{
  "siqiy": "1.0",
  "id": "doc-uuid",
  "meta": { "title": "Kühlkreis Anlage A", "author": "...", "created": "..." },
  "units": { "length": "mm", "pressure": "bar", "temperature": "°C" },
  "pages": [
    {
      "id": "page-uuid",
      "name": "Übersicht",
      "cells": { "Width": 2970, "Height": 2100 },
      "shapes": []
    }
  ],
  "stencils": [],
  "scripts": []
}
```

### Git-Diffs

Eine einzelne Wertänderung erzeugt einen minimalen, lesbaren Diff:

```diff
- "value": "off",
+ "value": "on",
```

Eine Formeländerung:

```diff
- "formula": "Self.Height * 1.5",
+ "formula": "Self.Height * 2.0",
- "value": 60.0,
+ "value": 80.0,
```

---

## 19. Stencil-Format

Ein Stencil ist eine Bibliothek von Master-Shapes – die Quelle der Wahrheit für alle Instanzen die darauf referenzieren.

### Designziele

- **Eigenständige Datei** – ein Stencil ist ohne das Tool lesbar und verwendbar
- **Versionierbar** – Git-kompatibel, jede Änderung nachvollziehbar
- **Verteilbar** – als URL, als lokale Datei, als npm-Paket
- **Typisiert** – definiert nicht nur Shapes sondern deren Typ-Hierarchie
- **Dokumentiert** – jedes Shape trägt seine eigene Beschreibung

### Stencil-Dateistruktur

Ein Stencil ist ein Ordner (oder ZIP) mit folgender Struktur:

```
pid-standard/
├── stencil.json          # Metadaten und Typ-Hierarchie
├── shapes/
│   ├── kreiselpumpe.json # Master-Shape Definition
│   ├── absperrventil.json
│   ├── behaelter.json
│   └── ...
├── geometry/
│   ├── kreiselpumpe.svg  # Geometrie-Definition (SVG)
│   └── ...
├── scripts/
│   └── validierung.ts    # Framework-Scripts
└── README.md             # Menschenlesbare Dokumentation
```

### stencil.json

```json
{
  "siqiy-stencil": "1.0",
  "id": "pid-standard",
  "name": "P&ID Standard (ISO 10628)",
  "version": "2.1.0",
  "author": "Siqiy Community",
  "license": "MIT",
  "description": "Standardschablone für Rohrleitungs- und Instrumentenfliessbilder nach ISO 10628",
  "tags": ["P&ID", "Verfahrenstechnik", "ISO 10628"],
  "types": {
    "Komponente": {
      "description": "Basistyp für alle physischen Komponenten",
      "props": {
        "bezeichnung": { "type": "string",  "label": "Bezeichnung", "required": true },
        "hersteller":  { "type": "string",  "label": "Hersteller",  "required": false },
        "baujahr":     { "type": "number",  "label": "Baujahr",     "unit": "Jahr" }
      }
    },
    "Rohrkomponent": {
      "extends": "Komponente",
      "description": "Alle Komponenten die in Rohrleitungen eingebaut werden",
      "props": {
        "nennweite":   { "type": "number",  "label": "Nennweite",   "unit": "DN" },
        "nenndruck":   { "type": "number",  "label": "Nenndruck",   "unit": "PN" },
        "werkstoff":   { "type": "string",  "label": "Werkstoff" }
      }
    },
    "Pumpe": {
      "extends": "Rohrkomponent",
      "description": "Pumpen aller Art",
      "props": {
        "foerdermenge":  { "type": "number", "label": "Fördermenge",  "unit": "m3/h" },
        "foerderhoehe":  { "type": "number", "label": "Förderhöhe",   "unit": "m" },
        "antriebsleistung": { "type": "number", "label": "Antriebsleistung", "unit": "kW" }
      }
    },
    "Kreiselpumpe": {
      "extends": "Pumpe",
      "description": "Kreiselpumpen (Radial-, Axial-, Diagonalpumpen)"
    }
  },
  "shapes": [
    "shapes/kreiselpumpe.json",
    "shapes/absperrventil.json",
    "shapes/behaelter.json"
  ]
}
```

### Master-Shape Definition

```json
{
  "id": "kreiselpumpe",
  "name": "Kreiselpumpe",
  "type": "Kreiselpumpe",
  "description": "Kreiselpumpe nach ISO 10628-2, Symbol KP",
  "norm": "ISO 10628-2",
  "geometry": "geometry/kreiselpumpe.svg",
  "cells": {
    "Width":     { "formula": "Self.Height * 1.2", "default": 48.0 },
    "Height":    { "formula": null,                 "default": 40.0 },
    "LocPinX":   { "formula": "Self.Width * 0.5",  "default": 24.0 },
    "LocPinY":   { "formula": "Self.Height * 0.5", "default": 20.0 },
    "FillColor": {
      "formula": "Self.Prop.status == 'fault' ? '#CC0000' : Self.Prop.status == 'off' ? '#888888' : '#FFFFFF'",
      "default": "#FFFFFF"
    }
  },
  "props": {
    "status": {
      "type": "enum",
      "options": ["on", "off", "fault"],
      "default": "off",
      "label": "Betriebszustand"
    }
  },
  "ports": {
    "saugstutzen": {
      "x": { "formula": "0" },
      "y": { "formula": "Self.Height / 2" },
      "direction": "left",
      "label": "Saugstutzen",
      "accepts": ["fluid-line", "gas-line"]
    },
    "druckstutzen": {
      "x": { "formula": "Self.Width" },
      "y": { "formula": "Self.Height / 2" },
      "direction": "right",
      "label": "Druckstutzen",
      "accepts": ["fluid-line", "gas-line"]
    }
  },
  "validation": [
    {
      "rule": "Self.Prop.nennweite > 0",
      "severity": "warning",
      "message": "Nennweite sollte angegeben werden"
    },
    {
      "rule": "Self.Prop.foerdermenge <= 5000",
      "severity": "error",
      "message": "Fördermenge überschreitet zulässigen Bereich"
    }
  ]
}
```

### Binding-Modell: Referenz vs. Kopie

Wenn ein Master-Shape in eine Zeichnung eingefügt wird, entsteht eine Instanz mit explizitem Binding-Status:

```
Stencil (Quelle der Wahrheit)
    │
    ├── Instanz A  [binding: "reference"]
    │   Erbt alles vom Master, lokale Overrides möglich
    │   Stencil-Update → Instanz aktualisiert sich
    │
    ├── Instanz B  [binding: "copy"]
    │   Vollständige lokale Kopie, entkoppelt
    │   Stencil-Update → keine Auswirkung
    │
    └── Instanz C  [binding: "reference", status: "outdated"]
        Master hat sich geändert, Update ausstehend
        Siqiy zeigt Hinweis, Nutzer entscheidet
```

### Stencil-Verteilung

Stencils können auf drei Arten referenziert werden:

```json
"stencils": [
  { "id": "pid-standard",   "source": "https://stencils.siqiy.io/pid-standard@2.1.0" },
  { "id": "mein-framework", "source": "file://./stencils/mein-framework" },
  { "id": "firma-intern",   "source": "npm:@meinefirma/siqiy-stencils@1.0.0" }
]
```

Dies ermöglicht firmenweite Stencils die zentral gepflegt werden – ändert sich ein Symbol, aktualisieren sich alle referenzierenden Zeichnungen weltweit.

---

## 20. ShapeSheet-Editor und Fehlerbehandlung

Der ShapeSheet-Editor ist das wichtigste UI-Element für den Framework-Builder. Er ist nicht nur ein Textfeld – er ist eine intelligente Entwicklungsumgebung für Formeln.

### Smarter Cell-Editor

Die Formel-Syntax ist intern präzise und case-sensitiv – aber der Editor schluckt Ungenauigkeiten und normalisiert sie transparent:

```
# Nutzer tippt (locker, case-insensitiv):
self.width = self.height * 1.5

# Editor normalisiert beim Verlassen der Cell:
Self.Width = Self.Height * 1.5

# Nutzer tippt:
fillcolor = pumpe3.prop.status = "on" ? green : red

# Editor erkennt Muster, schlägt vor:
FillColor = Shape("Pumpe3").Prop.status == "on" ? Green : Red
            ↑ Hinweis: "Meintest du Shape("Pumpe3")?"
```

**Was der Editor leistet:**

- **Auto-Normalisierung** – Gross-/Kleinschreibung wird beim Verlassen der Cell korrigiert, nicht beim Tippen
- **Fuzzy Shape-Erkennung** – `pumpe3` wird zu `Shape("Pumpe3")` aufgelöst wenn eindeutig
- **Autocomplete** – alle verfügbaren Shapes, Properties, Funktionen als Vorschlag
- **Live-Validierung** – Syntaxfehler, fehlende Referenzen, Typ-Konflikte sofort sichtbar
- **Formel-Erklärung** – Hover zeigt in Prosa was die Formel tut: *„Breite ist 1.5× die Höhe, aktuell 60px"*
- **Abhängigkeits-Visualisierung** – welche Cells hängen von dieser ab, welche beeinflusst sie
- **KI-Vorschläge** – *„Diese Pumpe hat keinen statusbasierten FillColor – soll ich einen vorschlagen?"*

Die Ergonomie beim Editieren hat Priorität. Die Syntax ist das interne Format – der Editor ist die menschliche Schnittstelle dazu.

### Fehlermodell: #ERR statt #REF

Wie in Visio bleibt der letzte gültige Wert immer erhalten und aktiv – die Zeichnung funktioniert weiter. Nur der Editor zeigt den Fehler deutlich, ohne die Arbeit zu blockieren.

```json
// Normalzustand
{ "formula": "Self.Height * 1.5", "value": 60.0, "state": "ok" }

// Referenz fehlt – Shape wurde gelöscht
{ "formula": "Shape(#a3f9).Prop.breite", "value": 60.0, "state": "#ERR:REF",
  "error": "Shape 'Pumpe3' nicht gefunden" }

// Division durch Null
{ "formula": "Page.Width / Self.Prop.anzahl", "value": 100.0, "state": "#ERR:DIV",
  "error": "Division durch Null (anzahl = 0)" }

// Zirkelreferenz
{ "formula": "Self.Height", "value": 40.0, "state": "#ERR:CIRC",
  "error": "Zirkelreferenz: Width → Height → Width" }
```

**Fehlercodes:**

| Code | Bedeutung |
|---|---|
| `#ERR:REF` | Referenz nicht gefunden (Shape, Port, Property) |
| `#ERR:DIV` | Division durch Null |
| `#ERR:TYPE` | Typ-Konflikt in Formel |
| `#ERR:CIRC` | Zirkelreferenz im Dependency-Graph |
| `#ERR:SYN` | Syntaxfehler in der Formel |
| `#ERR:UNIT` | Inkompatible Einheiten |
| `#ERR:LOCK` | Versuch eine gesperrte Cell zu überschreiben |

Der letzte gültige `value` bleibt immer erhalten. Die Zeichnung bleibt funktionsfähig. `#ERR` ist klarer als Visios `#REF` – es signalisiert einen Fehler, nicht nur eine fehlende Referenz.

---

## 21. Delta-Speicherung

### Das Prinzip

Shapes in einer Zeichnung sind meistens Instanzen eines Masters – sie unterscheiden sich nur in wenigen Cells vom Ur-Shape. Siqiy speichert intern nur diese Differenz, nicht die vollständige Shape.

Ein P&ID mit 200 gleichartigen Ventilen speichert nicht 200× die komplette Geometrie – sondern 200× nur das was abweicht, typischerweise Name, Position und ein paar Properties.

### Implementierung: Laufzeit vs. Disk

Die Delta-Speicherung ist eine transparente Optimierung – der Nutzer und die API merken nichts davon:

```
Zur Laufzeit (im Speicher):    vollständige, aufgelöste Objekte
Beim Speichern (auf Disk):     nur Deltas gegen den Master
```

```json
// Was auf Disk steht (kompakt)
{
  "id": "a3f9",
  "name": "Pumpe3",
  "master": "stencil://pid-standard/kreiselpumpe",
  "delta": {
    "cells": {
      "FillColor": { "formula": "Self.Prop.status == 'on' ? Green : Red", "value": "#00AA00" }
    },
    "props": {
      "bezeichnung": { "value": "P-101" },
      "druck":        { "value": 8.5 }
    }
  }
}

// Was zur Laufzeit im Speicher ist (vollständig aufgelöst)
{
  "id": "a3f9",
  "cells": {
    "Width":     { "formula": "Self.Height * 1.2", "value": 48.0 },
    "Height":    { "formula": null, "value": 40.0 },
    "FillColor": { "formula": "Self.Prop.status == 'on' ? Green : Red", "value": "#00AA00" }
  }
}
```

Beim Laden löst Siqiy das Delta automatisch auf. Beim Speichern berechnet Siqiy das Delta automatisch. Die Formel-Engine und der Renderer arbeiten immer mit vollständigen Objekten.

### Kollaboration und Deltas

Yjs synchronisiert immer das vollständig aufgelöste Objekt – nicht das Delta. Delta-Berechnung ist ausschliesslich ein Serialisierungsschritt. Das vermeidet Komplexität beim Konfliktlösen: Yjs muss nie wissen was vom Master kommt und was lokal ist.

### Wenn der Master fehlt

```json
{
  "state": "#ERR:REF",
  "error": "Master 'stencil://pid-standard/kreiselpumpe' nicht gefunden",
  "fallback": "last-known-full"
}
```

Siqiy speichert beim letzten erfolgreichen Laden einen Snapshot des vollständig aufgelösten Objekts als Fallback. Fehlt der Master, arbeitet die Zeichnung mit diesem Snapshot weiter – mit sichtbarem Hinweis, aber ohne Funktionsverlust.



## 22. Stil-System

Stile in Siqiy sind CSS-artig – sie definieren wiederverwendbare Erscheinungsbilder die regelbasiert und automatisch auf Shapes angewendet werden. Das Kernprinzip ist Cascade: spezifischere Regeln überschreiben allgemeinere, mehrere Regeln können gleichzeitig auf verschiedene Properties wirken.

### Vier Cascade-Ebenen

```
Ebene 1: Basis-Stil       (Dokument-Default, wie CSS reset)
Ebene 2: Typ-Stil         (am Master-Shape definiert)
Ebene 3: Regel-Stil       (datengetrieben, automatisch)
Ebene 4: Direkte Zuweisung (manuell, höchste Priorität)
```

### Praxisbeispiel: Flowsheet-Stile

Ein Ventil erhält sein Erscheinungsbild vollautomatisch aus seinen Properties – ohne dass der Zeichner etwas manuell einstellen muss:

```json
{
  "styles": {
    "medium.produkt":   { "cells": { "FillColor": "#CC0000" } },
    "medium.wasser":    { "cells": { "FillColor": "#0055CC" } },
    "medium.dampf":     { "cells": { "FillColor": "#FF8800" } },
    "medium.cip":       { "cells": { "FillColor": "#00AA55" } },

    "wichtigkeit.probenahme": {
      "cells": { "LineWeight": 0.5, "CornerRadius": 0, "TextVisible": false }
    },
    "wichtigkeit.hauptlinie": {
      "cells": { "LineWeight": 1.5 }
    },

    "geometrie.1D": { "cells": { "CornerRadius": 0 } },
    "geometrie.2D": { "cells": { "CornerRadius": 3 } }
  },

  "style-rules": [
    { "condition": "Self.Prop.medium == 'Produkt'",    "apply": "medium.produkt" },
    { "condition": "Self.Prop.medium == 'Wasser'",     "apply": "medium.wasser" },
    { "condition": "Self.Prop.medium == 'Dampf'",      "apply": "medium.dampf" },
    { "condition": "Self.Prop.medium == 'CIP'",        "apply": "medium.cip" },
    { "condition": "Self.Prop.wichtigkeit == 'Probenahme'", "apply": "wichtigkeit.probenahme" },
    { "condition": "Self.Prop.geometrie == '1D'",      "apply": "geometrie.1D" },
    { "condition": "Self.Prop.geometrie == '2D'",      "apply": "geometrie.2D" }
  ]
}
```

Ergebnis für `Ventil.123` (`medium=Produkt`, `wichtigkeit=Probenahme`, `geometrie=1D`):

```
medium.produkt        → FillColor:    #CC0000
wichtigkeit.probenahme → LineWeight:   0.5mm
                        CornerRadius: 0
                        TextVisible:  false
geometrie.1D          → CornerRadius: 0  (redundant, schadet nicht)
```

Mehrere Regeln greifen gleichzeitig auf verschiedene Properties. Greifen zwei Regeln auf dieselbe Property, gewinnt die spezifischere – oder die spätere in der Liste, konfigurierbar pro Framework.

### Stile und Views

Ein View kann Stil-Regeln selektiv aktivieren oder deaktivieren. Ein Druckplan schaltet z.B. alle farbbasierten Regeln aus und wendet stattdessen einen Schwarzweiss-Stil an.

### Stile im Datenmodell

Stile leben primär im Stencil – sie gehören zum Framework, nicht zur Zeichnung. Eine Zeichnung kann zusätzliche lokale Stile definieren, aber die Framework-Stile kommen aus dem Stencil und propagieren bei Updates automatisch.

---

## 23. Layer-System

Layer in Siqiy verbinden AutoCAD-Präzision mit moderner Tag-Semantik. Shapes gehören mehreren Layern gleichzeitig an – Layer sind keine Schubladen sondern Klassifikationen.

### Hierarchische Layer mit Custom Properties

```json
{
  "layers": {
    "architektur": {
      "label": "Architektur",
      "color": "#333333",
      "props": { "gewerk": "Bau" },
      "children": {
        "waende":   { "label": "Wände" },
        "tueren":   { "label": "Türen & Fenster" },
        "decken":   { "label": "Decken" }
      }
    },
    "elektro": {
      "label": "Elektro",
      "color": "#FFAA00",
      "props": { "gewerk": "Elektro", "auftragnehmer": "Elektro GmbH" },
      "children": {
        "starkstrom": {
          "label": "Starkstrom",
          "children": {
            "steckdosen": { "label": "Steckdosen" },
            "verteiler":  { "label": "Verteiler" }
          }
        },
        "schwachstrom": {
          "label": "Schwachstrom",
          "children": {
            "datenkabel": { "label": "Datenkabel" },
            "telefon":    { "label": "Telefon" }
          }
        }
      }
    },
    "moebel": {
      "label": "Möbel",
      "color": "#00AA55",
      "children": {
        "buero":       { "label": "Büro" },
        "besprechung": { "label": "Besprechung" }
      }
    }
  }
}
```

### Shapes mit mehreren Layer-Zugehörigkeiten

```json
{
  "id": "a3f9",
  "name": "Schreibtisch-42",
  "layers": ["moebel.buero", "elektro.schwachstrom.datenkabel"]
}
```

Dieser Schreibtisch erscheint im Möbelplan *und* im Datenkabel-Plan. Er hat eine primäre Zugehörigkeit (`moebel.buero`) und eine sekundäre (`elektro.schwachstrom.datenkabel`) – beide vollwertig.

### Views: gespeicherte Ansichten

Views sind das Herzstück des Layer-Systems – sie kombinieren Layer-Sichtbarkeit, Transparenz, Farb-Overrides und Druckbarkeit zu benannten, wiederverwendbaren Ansichten:

```json
{
  "views": {
    "elektroplan": {
      "label": "Elektroplan",
      "layers": {
        "architektur":          { "visible": true,  "opacity": 0.3, "locked": true },
        "elektro":              { "visible": true,  "opacity": 1.0 },
        "elektro.starkstrom":   { "visible": true,  "color-override": "#FF6600" },
        "elektro.schwachstrom": { "visible": true,  "color-override": "#0066FF" },
        "moebel":               { "visible": false }
      },
      "print": true,
      "scale": "1:50"
    },
    "moebelplan": {
      "label": "Möbelplan",
      "layers": {
        "architektur": { "visible": true,  "opacity": 1.0 },
        "moebel":      { "visible": true,  "opacity": 1.0 },
        "elektro":     { "visible": false },
        "sanitaer":    { "visible": false }
      }
    },
    "druckversion": {
      "label": "Druckversion SW",
      "layers": {
        "*": { "color-override": "monochrome" }
      },
      "style-rules": "none",
      "print": true,
      "exclude-layers": ["hilfslinien", "massketten.intern"]
    }
  }
}
```

Der `*`-Wildcard macht alle Layer mit einem Eintrag monochrom. `style-rules: none` schaltet alle farbbasierten Stil-Regeln für diesen View ab.

### Layer-Properties abfragen

Da Layer Custom Properties haben, können Formeln und Scripts sie abfragen:

```
Self.Prop.auftragnehmer = Layer("elektro").props.auftragnehmer
```

Und Views können nach Layer-Properties gefiltert werden:

```json
{ "condition": "layer.props.gewerk == 'Elektro'", "visible": true }
```

### Was Visio nicht kann – was Siqiy hinzufügt

| Feature | Visio | Siqiy |
|---|---|---|
| Hierarchische Layer | ✗ | ✓ |
| Shape in mehreren Layern | ✗ | ✓ |
| Custom Properties pro Layer | ✗ | ✓ |
| Gespeicherte Views | Rudimentär | ✓ vollständig |
| Farb-Override pro View | ✗ | ✓ |
| Layer per Formel abfragen | ✗ | ✓ |
| Stil-Regeln pro View an/aus | ✗ | ✓ |

*Dieses Dokument ist ein lebendes Arbeitsdokument. Es wächst mit dem Projekt.*
