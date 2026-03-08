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

### Architektur-Entscheidung: Rendering

Die Wahl der Rendering-Engine ist die fundamentalste Weichenstellung des Projekts.

| Ansatz | Stärken | Schwächen für Siqiy |
|---|---|---|
| **DOM/SVG** (tldraw, React Flow) | Accessibility, CSS-Styling, DevTools | Performance bei >500 Objekten bricht ein |
| **Canvas 2D** (Konva, Fabric) | Schnell, pixelgenaue Kontrolle | Kein natives Hit-Testing, keine Accessibility |
| **WebGL** (PixiJS) | Extrem schnell, 10.000+ Objekte | Komplex, Text-Rendering aufwändig |
| **Hybrid** | Beste Performance + UX | Komplexere Architektur |

**Empfehlung: Hybrid-Ansatz**

- **WebGL/Canvas (PixiJS)** für das eigentliche Rendering der Shapes (Performance)
- **SVG-Overlay** für Connectors und Text (Präzision, Skalierung)
- **DOM-Layer** ganz oben für UI-Elemente, ShapeSheet-Editor, Handles

### Warum nicht tldraw?

tldraw bietet gute UX, ist aber für Siqiy zu sehr eine fertige Meinung:

- Kein echtes Port/Anker-System
- Kein orthogonales Connector-Routing
- Keine Metadaten-Schicht hinter Shapes
- Die interne Architektur ist schwer erweiterbar ohne Fork
- Zu sehr auf Whiteboard-UX optimiert

### Datenmodell

CRDTs und reaktive Formel-Engine schließen sich nicht aus – Siqiy braucht beide:

```
┌─────────────────────────────────────┐
│  CRDT-Layer (Yjs)                   │  ← Kollaboration, Sync
│  YMap pro Shape, YArray für Seiten  │
├─────────────────────────────────────┤
│  Reaktiver Dependency-Graph         │  ← ShapeSheet-Formeln
│  (HyperFormula oder custom)         │
├─────────────────────────────────────┤
│  Semantischer Metadaten-Layer       │  ← Shape-Properties, Typen
│  (JSON-Schema pro Shape-Typ)        │
└─────────────────────────────────────┘
```

**Kritisches Risiko:** CRDTs und reaktive Formel-Graphen zusammen sind komplex. Wenn eine Formel sich auf eine Shape bezieht die gerade jemand anderes bearbeitet, entsteht ein Konflikt auf Formel-Ebene, nicht nur auf Daten-Ebene. Das muss von Anfang an mitgedacht werden.

### Connector-Routing

Das orthogonale Routing ist eine der härtesten technischen Nüsse:

- **A\*-Algorithmus** auf einem Gitter – Standard, aber naiv bei vielen Objekten langsam
- **Visibility Graph** – präziser, aber O(n²) Komplexität
- **draw.io Routing-Code** – MIT-lizenziert, praxiserprobt, wiederverwendbar

draw.io hat dieses Problem bereits gelöst. Der Routing-Code ist MIT-lizenziert und eine ernsthafte Basis.

### Empfohlener Stack

```
┌─────────────────────────────────────────────┐
│  UI-Layer                                   │
│  React + TypeScript                         │
│  shadcn/ui für Komponenten                  │
├─────────────────────────────────────────────┤
│  Canvas-Engine                              │
│  PixiJS (WebGL) für Shape-Rendering         │
│  SVG-Overlay für Connectors & Text          │
├─────────────────────────────────────────────┤
│  Routing-Engine                             │
│  draw.io Routing-Algorithmen (MIT)          │
│  Ports/Anker custom implementiert           │
├─────────────────────────────────────────────┤
│  Formel-Engine                              │
│  HyperFormula (reaktiver Dep-Graph)         │
│  Pyodide für Python-Scripting               │
├─────────────────────────────────────────────┤
│  State & Kollaboration                      │
│  Yjs (CRDT) + WebRTC / WebSocket            │
│  Zustand für lokalen UI-State               │
├─────────────────────────────────────────────┤
│  Desktop                                    │
│  Tauri (Rust) – web-first, desktop-ready    │
└─────────────────────────────────────────────┘
```

### Strategie

**Web-first, Desktop-ready.** Die App als Web-App bauen, von Anfang an so architekturiert dass Tauri-Wrapping trivial ist. Lokale Datenhaltung als Default, keine Cloud-Abhängigkeiten im Core.

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

*Dieses Dokument ist ein lebendes Arbeitsdokument. Es wächst mit dem Projekt.*
