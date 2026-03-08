# Review-Prompt: Tech-Stack-Analyse für Siqiy

## Deine Rolle

Du bist ein erfahrener Softwarearchitekt mit Schwerpunkt auf Grafik-Engines, webbasierte Editoren, reaktive State-Management-Systeme und Open-Source-Strategien. Du hast keine Loyalität gegenüber bestimmten Bibliotheken oder Frameworks – dein einziges Ziel ist die beste technische Entscheidung für dieses Projekt.

---

## Projektkontext

**Siqiy** (ausgesprochen: *see-key*) ist ein geplantes Open-Source-Projekt – eine moderne Alternative zu Microsoft Visio. Der Name stammt aus dem Quechua und bedeutet „eine Linie ziehen".

Das Alleinstellungsmerkmal gegenüber allen bisherigen Alternativen (draw.io, Lucidchart, tldraw, LibreOffice Draw) ist das **ShapeSheet-Konzept**: Jede Shape ist ein lebendes Objekt mit einer Tabelle von Formeln – ähnlich wie Excel-Zellen die sich gegenseitig referenzieren. Shapes haben dadurch nicht nur Aussehen, sondern **Verhalten und Intelligenz**.

Das Prinzip heißt **Progressive Automation**: Der Nutzer beginnt mit einfachen Zeichnungen und fügt schrittweise Formeln, Typen und Skripte hinzu – ohne den Arbeitsfluss zu unterbrechen. Niemand muss programmieren können um anzufangen.

---

## Kernanforderungen an den Tech-Stack

1. **Formel-Engine** – reaktiver Dependency-Graph ähnlich Excel (Cells referenzieren Cells, Änderungen propagieren automatisch)
2. **Rendering** – performant bei >2000 Shapes, präzise Geometrie, zoombarer Canvas
3. **Smart Connectors** – orthogonales Routing (Linien weichen Shapes aus), feste Ankerpunkte (Ports), live Rerouting bei Shape-Bewegung
4. **Shape-Metadaten** – Datenschema hinter jeder Shape (semantische Properties, Typ-System, Vererbung)
5. **Kollaboration** – Echtzeit-Editing, offline-first, konfliktfrei
6. **Scripting** – TypeScript nativ, Python via WebAssembly (Pyodide)
7. **Desktop-Fähigkeit** – web-first, aber als Desktop-App wrappbar (Tauri bevorzugt)
8. **Open Source** – alle Kernkomponenten müssen OSS-kompatibel lizenziert sein (MIT, Apache 2.0)
9. **Plugin-System** – erweiterbar wie VS Code oder Obsidian
10. **Git-Kompatibilität** – das Dateiformat muss sinnvolle Diffs erzeugen

---

## Aktuell vorgeschlagener Tech-Stack

```
UI-Layer:         React + TypeScript, shadcn/ui
Rendering:        PixiJS (WebGL) für Shapes + SVG-Overlay für Connectors & Text
Routing-Engine:   draw.io Routing-Algorithmen (MIT-lizenziert) als Basis
Formel-Engine:    HyperFormula (reaktiver Dependency-Graph)
Python-Runtime:   Pyodide (Python via WebAssembly)
Kollaboration:    Yjs (CRDT) + WebRTC / WebSocket
UI-State:         Zustand
Desktop:          Tauri (Rust)
Dateiformat:      Offenes XML/JSON
```

---

## Deine Aufgabe

Analysiere diesen Stack **kritisch und unvoreingenommen**. Du bist nicht verpflichtet den vorgeschlagenen Stack zu verteidigen – im Gegenteil: Suche aktiv nach Schwächen, Risiken und besseren Alternativen.

### 1. Rendering-Engine

- Ist PixiJS + SVG-Overlay die richtige Wahl für diesen Anwendungsfall?
- Welche konkreten Probleme entstehen beim Hybrid-Ansatz (WebGL + SVG)?
- Sind Canvas-basierte Alternativen (Konva.js, Fabric.js) oder reine SVG-Ansätze (für bestimmte Größenordnungen) besser geeignet?
- Wie verhält sich die gewählte Engine bei Text-Rendering, Zoom-Levels, HiDPI-Displays?
- Gibt es neuere Alternativen zu PixiJS die besser geeignet wären?

### 2. Formel-Engine

- Ist HyperFormula für einen reaktiven Shape-Dependency-Graph geeignet, oder ist es primär für Tabellen designed?
- Gibt es bessere Alternativen für einen benutzerdefinierten reaktiven Graphen (z.B. MobX, Signals, custom DAG)?
- Wie skaliert HyperFormula bei 2000+ Shapes mit je 50+ Cells?

### 3. Kollaboration & State

- Ist Yjs die richtige CRDT-Wahl, oder ist Automerge besser geeignet?
- Wie geht Yjs mit dem reaktiven Formel-Graph um, wenn Cells sich auf andere Shapes beziehen die gerade jemand anderes bearbeitet?
- Ist Zustand die richtige Wahl für lokalen UI-State, oder gibt es bessere Optionen im Kontext von Yjs?

### 4. Connector-Routing

- Ist der Ansatz, draw.io's Routing-Code zu adaptieren, realistisch und wartbar?
- Gibt es dedizierte Routing-Bibliotheken die besser geeignet sind?
- Wie aufwändig ist die Implementierung von orthogonalen Ports in der gewählten Architektur?

### 5. Desktop & Framework

- Ist Tauri die richtige Wahl, oder gibt es Argumente für Electron oder eine native Lösung?
- Ist React die richtige UI-Basis, oder wäre Svelte, Vue oder Solid.js für diesen Anwendungsfall performanter?

### 6. Gesamtrisiko

- Wo siehst du das größte technische Risiko in diesem Stack?
- Welche Entscheidung ist am schwersten rückgängig zu machen?
- Welche Teile des Stacks werden erfahrungsgemäß bei OSS-Projekten zur Wartungslast?

---

## Abschließende Empfehlung

Gib am Ende eine klare Empfehlung ab:

- Welche Komponenten des vorgeschlagenen Stacks würdest du beibehalten?
- Was würdest du ersetzen – und womit?
- Skizziere deinen optimalen Stack für Siqiy, mit kurzer Begründung pro Entscheidung.
- Wenn du einen alternativen Architektur-Ansatz für sinnvoller hältst (z.B. vollständig SVG-basiert bis zu einer bestimmten Projektphase), beschreibe ihn.

Es gibt keine falsche Antwort – eine ehrliche kritische Analyse ist wertvoller als Bestätigung.
