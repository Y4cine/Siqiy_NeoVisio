# Siqiy – Runde 2: Synthese und Konsensfindung

## Kontext

Du hast bereits eine kritische Analyse des vorgeschlagenen Tech-Stacks für **Siqiy** geliefert. Nun hast du Zugang zu den Antworten mehrerer anderer KI-Modelle, die denselben Auftrag erhalten haben.

Deine Aufgabe ist nicht, deine frühere Antwort zu verteidigen. Deine Aufgabe ist, gemeinsam mit den anderen Analysen die **realistischste, am besten umsetzbare Lösung** zu finden.

---

## Die Antworten deiner Kollegen

*(Die vollständigen Analysen der anderen Modelle sind im Anhang beigefügt.)*

---

## Wichtige Vorbemerkung: Was "beste Lösung" hier bedeutet

Das Ziel ist **nicht** technische Perfektion. Das Ziel ist:

> **Welcher Stack maximiert die Wahrscheinlichkeit, dass Siqiy tatsächlich gebaut, veröffentlicht und von einer wachsenden Community weiterentwickelt wird?**

Das bedeutet konkret:

- Ein einfacherer Stack der von einem einzelnen Entwickler bootstrappbar ist, schlägt einen optimalen Stack der ein 5-köpfiges Kernteam voraussetzt
- Bewährte Bibliotheken mit aktiver Community schlagen elegantere Lösungen mit Wartungsrisiko
- Klare Architekturentscheidungen schlagen flexible aber komplexe Designs
- "Gut genug für Phase 1" ist wichtiger als "perfekt für Phase 4"

---

## Deine Aufgabe in dieser Runde

### 1. Konsens-Kartierung

Identifiziere für jede Stack-Komponente den Grad der Übereinstimmung:

- ✅ **Klarer Konsens** – alle oder fast alle Analysen kommen zum selben Schluss
- ⚠️ **Begründete Meinungsverschiedenheit** – verschiedene Antworten, aber alle mit validen Argumenten
- ❌ **Klare Ablehnung** – eine Komponente wird von mehreren Analysen übereinstimmend als problematisch eingestuft

Gehe dabei durch folgende Komponenten:
- Rendering-Engine (PixiJS + SVG Hybrid)
- Formel-Engine (HyperFormula)
- Kollaboration (Yjs)
- Connector-Routing (draw.io Code)
- UI-Framework (React)
- Desktop (Tauri)
- State-Management (Zustand)
- Scripting (TypeScript + Pyodide)

### 2. Tradeoff-Analyse bei Meinungsverschiedenheiten

Wo es begründete Meinungsverschiedenheiten gibt: Beschreibe den echten Tradeoff. Nicht "wer hat recht", sondern: **Welche Priorität führt zu welcher Entscheidung?**

Beispiel: "Wer Performance bei >5000 Shapes priorisiert, wählt X. Wer Entwicklungsgeschwindigkeit und einfachere Architektur priorisiert, wählt Y."

### 3. Phasenmodell

Berücksichtige, dass Siqiy in Phasen gebaut wird. Manche Entscheidungen sind nur für Phase 1 (MVP) relevant, andere sind langfristige Architekturentscheidungen.

Trenne explizit:
- **Unwiderrufliche Entscheidungen** – Architekturwahl die später schwer zu ändern ist (z.B. Rendering-Paradigma, Datenmodell-Grundstruktur)
- **Reversible Entscheidungen** – Bibliotheken die später ausgetauscht werden können ohne Totalumbau

### 4. Das eine größte Risiko

Was ist nach Lektüre aller Analysen das **einzelne größte technische Risiko** für Siqiy – die Entscheidung die am wahrscheinlichsten scheitert oder das Projekt blockiert?

### 5. Dein optimaler Stack

Formuliere auf Basis aller Analysen – inklusive deiner eigenen – den Stack den du jetzt empfehlen würdest. 

Strukturiere ihn in zwei Varianten:

**Variante A – Konservativ (solo/kleines Team, maximale Realisierungswahrscheinlichkeit):**
Welcher Stack ist für eine einzelne Person oder ein 2-3 Personen Team am schnellsten zum ersten funktionierenden Prototyp?

**Variante B – Ambitioniert (Team vorhanden, langfristig optimal):**
Welcher Stack ist technisch am solidesten für die langfristige Vision, wenn genug Entwickler vorhanden sind?

Für jede Komponente: kurze Begründung, und ob sie in beiden Varianten gleich ist oder abweicht.

---

## Format

- Sei direkt und konkret
- Vermeide Wiederholungen aus deiner ersten Analyse
- Wenn du einer anderen Analyse zustimmst, sag es klar – kein Mehrwert in falscher Originalität
- Wenn du einer anderen Analyse widersprichst, begründe es mit einem Argument das in den anderen Analysen noch nicht genannt wurde
- Maximale Länge: das was nötig ist, nicht mehr

---

## Anhang: Analysen der anderen Modelle

# ChatGPT

Ich habe deinen vorgeschlagenen Stack gegen die Anforderungen aus dem Siqiy-Manifest gelesen – und die zentrale Idee ist stark: **nicht nur ein Zeichenprogramm, sondern ein ShapeSheet-basiertes Diagramm-System mit progressiver Automatisierung**. Genau daran scheitern die meisten Alternativen. 

Mein Gesamturteil vorweg: **Die grobe Richtung stimmt, aber zwei Entscheidungen sind im aktuellen Vorschlag brandgefährlich**:
**(1) HyperFormula** passt technisch nur halb und lizenzseitig schlecht,
**(2) der WebGL+SVG-Hybrid** ist leistungsfähig, aber architektonisch deutlich teurer als er auf dem Papier wirkt. PixiJS, Yjs und Tauri sind dagegen ziemlich vernünftige Kandidaten. ([hyperformula.handsontable.com][1])

## 1) Rendering-Engine

### PixiJS + SVG-Overlay: brauchbar, aber mit versteckter Steuer

PixiJS ist heute leistungsfähig, läuft auf WebGL und inzwischen optional auch WebGPU, und die Maintainer positionieren es klar als High-Performance-2D-Engine. Dazu kamen 2025/2026 weitere Verbesserungen beim Text-Stack. Rein für viele Shapes ist PixiJS also keine dumme Wahl. ([pixijs.com][2])

Der Haken ist der **Hybrid-Ansatz**. Zwei Rendering-Welten bedeuten fast immer doppelte Komplexität:

* zwei Koordinatensysteme,
* zwei Hit-Testing-Pfade,
* zwei Selektions-/Hover-Modelle,
* zwei Zoom-/Pixel-Snapping-Probleme,
* zwei Exportpfade,
* und besonders nervig: Text, Connector-Handles, Ports und Auswahlrahmen müssen millimetergenau mit dem WebGL-Layer synchron bleiben.

Das ist nicht unlösbar. Aber es ist genau die Sorte Problem, bei der man sich monatelang an “fast richtig” wundarbeitet.

### Was konkret schiefgehen kann

Der Hybrid-Ansatz ist besonders heikel bei:

* **Text**: WebGL-Text ist oft atlas-/bitmaplastig; SVG/DOM-Text ist semantisch schöner, aber dann lebt Text in einer anderen Welt als die Shape-Geometrie. Bei Zoom, Rotation, Subpixel-Snapping und HiDPI sieht man Inkonsistenzen schnell.
* **Connectoren**: Wenn Shapes in Pixi liegen und Connectoren in SVG, dann müssen Bounds, Ports, Hindernisse und Rerouting ständig über Layer-Grenzen synchronisiert werden.
* **Interaktion**: Selection box, resize handles, snap lines, hover feedback – alles will in derselben visuellen Realität leben. Zwei Realitäten sind Mist.

### Konva/Fabric/reines SVG?

* **Konva** ist ordentlich für interaktive 2D-Canvas-UIs und bewirbt ausdrücklich “thousands of shapes”, bietet Hit-Detection und Performance-Tipps. Für ein Whiteboard oder Editor-MVP ist das attraktiv. Aber bei sehr großer Dichte, komplexen Connectoren und präziser Diagrammgeometrie landet man oft doch bei Low-Level-Optimierung oder GPU-näheren Lösungen. ([Konva.js][3])
* **Fabric.js** ist eher “Canvas mit Objektmodell und Serialisierung”. Gut für Grafikeditoren, aber ich würde es für Siqiy nicht als Kern empfehlen. Zu viel Historie, zu wenig Fokus auf hochdynamische, semantische Diagramm-Engines. Text ist vorhanden, aber nicht der Grund, warum man damit eine Visio-Alternative gewinnt. ([fabricjs.com][4])
* **Reines SVG** würde ich **nicht** als Endlösung sehen, aber sehr wohl als **Phase-1-Architektur**. Bis einige hundert bis vielleicht niedrig vierstellige Objekte kann SVG/DOM extrem produktiv sein: Accessibility, Debuggability, CSS, native Textqualität, sauberer Export. Danach wird es eng. Dein eigenes Manifest sagt das im Kern schon. 

### Mein Urteil Rendering

Für das **langfristige Ziel** ist **PixiJS plausibel**.
Für das **erste belastbare Produkt** würde ich aber ernsthaft erwägen:

**Variante A – vorsichtig:**
SVG/DOM zuerst für Shapes + Connectoren + Text, später Renderer austauschbar machen.

**Variante B – ambitioniert, aber sauberer als Hybrid:**
PixiJS für fast alles Visuelle **inklusive Connectoren**, DOM nur für Editor-UI, Inspektoren und maybe text-edit overlays. Also lieber **ein dominanter Renderpfad** statt 50/50-Hybrid.

Der heutige Vorschlag “Shapes in Pixi, Connectoren/Text in SVG” ist technisch möglich, aber riecht nach spätem Architekturkater.

## 2) Formel-Engine

Hier wird es ernst. **HyperFormula ist der am meisten überschätzte Teil des vorgeschlagenen Stacks.**

HyperFormula ist ein leistungsfähiger Spreadsheet-Interpreter mit Dependency-Graph, Custom Functions und Performance-Tuning. Also: ja, man *kann* damit reaktive Berechnung bauen. Aber seine natürliche Heimat ist weiterhin **tabellarische Zelllogik**. ([hyperformula.handsontable.com][1])

Für Siqiy habt ihr aber nicht bloß Zellen, sondern:

* Shape-Instanzen,
* Vererbung/Typen,
* Referenz vs. Kopie,
* Ports,
* Connector-Topologie,
* ggf. Units,
* später Events/Skripte,
* und Formeln, die räumlich-semantisch quer über Dokumentobjekte laufen.

Das ist **kein Spreadsheet-Problem**, sondern ein **objektbezogener, semantischer DAG**.

### Der eigentliche Showstopper: Lizenz

HyperFormula ist laut Repo **GPLv3 oder proprietär**, und die Doku spricht von License Keys für Nutzung außerhalb Handsontable-Kontexten. Für ein Projekt, dessen Kernkomponenten MIT/Apache-kompatibel und OSS-freundlich sein sollen, ist das eine massive rote Flagge. ([GitHub][5])

Ganz unverblümt: **Wenn Siqiy permissiv und unkompliziert offen sein soll, fliegt HyperFormula raus.**
Nicht vielleicht. Raus.

### Technisch auch nicht ideal

Weitere Probleme:

* Referenzen in Shape-Namensräumen statt A1-Notation sind unnatürlich.
* Serialisierung stabiler Objekt-IDs ist kein natürlicher Erstbürger von HyperFormula. In Diskussionen wird sogar erwähnt, dass interne IDs nicht einfach persistent übernommen werden. ([GitHub][6])
* Spätestens bei Units, Shape-Typen und Topologie würdet ihr darum herumarchitektieren statt damit zu arbeiten.

### Bessere Richtung

Ich würde eine **eigene Formel-/Evaluation-Engine** bauen:

* eigener Parser,
* eigener symbolischer Referenzraum (`Shape("Pump3").Prop.Flow`, `Self.Width`, `Port("In").X`),
* eigener DAG,
* inkrementelle Re-Evaluation,
* Dirty-Propagation,
* später Unit-System als Typ-Layer.

Für UI-Reaktivität kann man intern mit Signals/MobX-artigen Konzepten arbeiten, aber **die Formelengine selbst** sollte domäneneigen sein, nicht an ein Spreadsheet geborgt.

## 3) Kollaboration & State

### Yjs vs Automerge

**Yjs** ist heute weiterhin eine sehr vernünftige Wahl. Es ist schnell, battle-tested, arbeitet mit Shared Types wie Map/Array/Text und ist stark im Editor-Ökosystem verankert. ([docs.yjs.dev][7])

**Automerge** ist ebenfalls stark, besonders aus der “local-first”-Perspektive, und macht bei einigen Datenmodellen konzeptionell eine sehr saubere Figur. Es betont Offline-First und lokale Primärkopien deutlich. ([automerge.org][8])

Meine praktische Einschätzung:

* **Yjs**, wenn ihr schnell zu kollaborativem Editing kommen wollt und gute Integrationen schätzt.
* **Automerge**, wenn ihr das Datenmodell sehr bewusst local-first, repository-artig und langfristig dokumentzentriert aufziehen wollt.

Für Siqiy würde ich **Yjs leicht bevorzugen** – aber **nicht** als Rechenmotor, sondern nur als **Dokument-/Sync-Layer**.

### Die wichtige Trennung

Der Fehler wäre, CRDT-Zustand und Formelzustand ineinander zu knoten.
Besser:

**Yjs = Source of truth für Dokumentdaten**
**Formula Engine = deterministische, lokale Ableitung daraus**

Also:

* CRDT speichert Zellformeln, Werte, Metadaten, Geometry-Inputs.
* Die Evaluationsengine baut lokal den Abhängigkeitsgraphen und berechnet Outputs.
* Bei Remote-Änderungen wird neu invalidiert und lokal neu gerechnet.

So vermeidet ihr “CRDT-Logik in der Formelengine”. Sonst baut ihr euch einen kleinen verteilten Nervenzusammenbruch.

### Zustand?

**Zustand** ist für lokalen UI-State völlig okay: Selection, Panels, Tool-Mode, transient drag state.
Ich würde es **behalten**, aber streng begrenzen:

* kein Dokumentkern,
* keine Formeltruth,
* keine Persistenzlogik.

Wenn ihr später merkt, dass React-Renderzyklen euch nerven, ist das kein Zustand-Problem, sondern ein falscher UI/Canvas-Schnitt.

## 4) Connector-Routing

### draw.io-Code übernehmen?

Realistisch? **Ja, aber nur teilweise.**
Wartbar? **Nur wenn ihr ihn als Referenz nehmt, nicht als heiligen Fremdkörper.**

Dein Manifest ist hier nüchtern: orthogonales Routing ist eine harte Nuss, und draw.io hat einen praxiserprobten Fundus. Das stimmt. 

Aber Copy-Paste-Architektur hat drei Risiken:

* fremde Datenmodelle und Annahmen,
* schwer lesbare Legacy-Heuristiken,
* Fork-Maintenance über Jahre.

Ich würde den draw.io-Ansatz **studieren, testen, extrahieren**, aber nicht blind als Core einbauen.

### Was ich stattdessen tun würde

Stufenmodell:

1. **Ports + Manhattan routing light**
2. **Obstacle avoidance auf Grid / coarse grid**
3. **Incremental rerouting** nur für betroffene Region
4. erst später raffiniertere Heuristiken / visibility graph hybrids

Der große Fehler wäre, im MVP “Smart Connectors wie Visio, aber besser” versprechen zu wollen. Das ist die Sorte Satz, die Teams innerlich verrotten lässt.

## 5) Desktop & UI-Framework

### Tauri

**Tauri ist für dieses Projekt vernünftig.** Es nutzt System-WebViews statt komplettes Chromium zu bundlen und positioniert sich explizit als klein, schnell und sicherheitsbewusst. ([Tauri][9])

Nachteile:

* Plattformspezifische WebView-Differenzen,
* Plugin-/Native-Bridge-Komplexität,
* Debugging nicht immer so bequem wie Electron.

Trotzdem: Für ein web-first Diagrammtool ist **Tauri die bessere Standardwahl** als Electron – solange ihr keine brutale Chromium-Konstanz braucht.

### React oder was anderes?

**React ist okay**, aber nicht automatisch optimal.

* **React**: großes Ökosystem, viele Contributor können einsteigen, gute Bibliothekslage.
* **Solid**: feinere Reaktivität, potenziell angenehmer für editorische High-Frequency-UIs.
* **Svelte**: sehr angenehm, aber kleineres Enterprise-/Editor-Ökosystem.
* **Vue**: ebenfalls solide, aber kein klarer Sieger hier.

Für ein OSS-Projekt ist **React + TypeScript** schlicht die pragmatischste Wahl. Nicht weil es technisch “am besten” wäre, sondern weil es die niedrigste Rekrutierungsreibung hat. Und OSS scheitert öfter an Maintainer-Energie als an theoretischer UI-Perfektion.

## 6) Lizenz- und OSS-Risiko

Hier muss man brutal ehrlich sein:

* **PixiJS**: MIT, gut. ([GitHub][10])
* **Yjs**: MIT-Kernteile, gut. ([GitHub][11])
* **HyperFormula**: **GPLv3/proprietär** → für euren Zielrahmen problematisch bis unpassend. ([GitHub][5])
* **Pyodide**: **MPL 2.0**, also offen, aber nicht so simpel-permissiv wie MIT/Apache. Wenn “nur MIT/Apache” wirklich hart gemeint ist, fällt Pyodide formal auch aus dem sauberen Raster. ([GitHub][12])

Das heißt: Der vorgeschlagene Stack ist **heute schon nicht sauber deckungsgleich mit Anforderung 8**.

## 7) Größtes technisches Risiko

Das größte Risiko ist **nicht** Tauri.
Auch nicht React.
Nicht einmal Yjs.

Das größte Risiko ist die **Kombination aus drei dynamischen Systemen gleichzeitig**:

1. reaktive Formelabhängigkeiten,
2. semantisches Typ-/Vererbungsmodell,
3. kollaborative CRDT-Änderungen.

Wenn diese drei Ebenen nicht hart voneinander getrennt sind, wird Siqiy zu einem Projekt, das unglaublich intelligent klingt und ständig an Randfällen stirbt.

Die am schwersten rückgängig zu machende Entscheidung ist daher nicht das UI-Framework, sondern das **interne Dokument- und Ausführungsmodell**:

* Wie werden Referenzen adressiert?
* Was ist persistent?
* Was ist abgeleitet?
* Was ist lokal transient?
* Wie verhalten sich Typen, Master, Overrides und Formeln zueinander?

Wenn das wackelt, hilft dir kein hübscher Renderer.

## Meine klare Empfehlung

### Beibehalten

* **React + TypeScript**: pragmatisch, contributor-freundlich.
* **PixiJS**: als langfristige Rendering-Basis plausibel.
* **Yjs**: sehr guter Sync-/CRDT-Kandidat.
* **Tauri**: vernünftige Desktop-Strategie.
* **JSON-basiertes, git-freundliches Dateiformat**: unbedingt.

### Ersetzen / ändern

* **HyperFormula ersetzen** → durch **eigene Formelengine + custom DAG**
* **Hybrid Pixi + SVG nicht als Default festzementieren** → entweder

  * am Anfang **komplett SVG/DOM**, oder
  * langfristig **Pixi-dominant** mit minimalem DOM-Overlay
* **draw.io-Routing-Code nicht einfach übernehmen** → als Referenz und Testbasis verwenden, aber Routing-Subsystem sauber selbst modellieren
* **Pyodide nicht als Kernabhängigkeit behandeln** → eher als optionale Capability/Plugin-Schicht, auch wegen Größe, Startup und Lizenzprofil. ([pyodide][13])

## Mein optimaler Stack für Siqiy

**Phase 1 – sauberes Fundament**

* UI: **React + TypeScript**
* Dokumentmodell: **eigene JSON-Domain-Modelle**
* Rendering: **SVG/DOM zuerst**
* Formelengine: **custom parser + custom DAG**
* Lokalstate: **Zustand**
* Kollaboration: **noch nicht voll**, aber Datenmodell Yjs-ready
* Desktop: **Tauri-ready**, noch optional

**Phase 2 – ernsthafte Editor-Phase**

* Rendering: **PixiJS renderer abstraction** einführen
* Connectoren: zunächst einfaches orthogonales Routing + Ports
* Kollaboration: **Yjs**
* Scripting: **TypeScript zuerst**, Python später

**Phase 3 – Plattform**

* PixiJS als Haupt-Renderer
* Yjs produktiv
* Tauri Desktop-Build
* Python via **Pyodide als Plugin/optional runtime**, nicht als harter Kern

## Kurzfassung ohne Höflichkeitszucker

Der aktuelle Vorschlag ist **visionär, aber an zwei Stellen zu bequem gedacht**:

* HyperFormula wirkt attraktiv, ist aber für Siqiy am Ende ein Fremdkörper **und lizenzseitig ein Problem**. ([GitHub][5])
* PixiJS ist okay, aber der **WebGL+SVG-Hybrid** ist kein Gratis-Lunch, sondern ein teures Wartungsdarlehen.

**Meine Entscheidung wäre daher:**

* React behalten
* PixiJS behalten, aber später sauber einführen
* Yjs behalten
* Tauri behalten
* HyperFormula raus
* eigene Formelengine rein
* Rendering in frühen Phasen simpler halten als die Vision es verführt

Was ich verworfen habe: Fabric als Kern, HyperFormula trotz Bequemlichkeit, und “wir nehmen draw.io-Code einfach und sind fertig” – das wäre zu schön, um wahr zu sein.
Zum Vertiefen würde ich als Nächstes das **interne Referenzmodell** spezifizieren: Namensraum, IDs, Formel-Syntax, Override-Regeln, abgeleitete vs. persistente Werte. Das ist der eigentliche Maschinenraum.

[1]: https://hyperformula.handsontable.com/guide/dependency-graph.html?utm_source=chatgpt.com "Dependency graph | HyperFormula (v3.2.0)"
[2]: https://pixijs.com/8.x/guides/getting-started/intro?utm_source=chatgpt.com "Introduction | PixiJS"
[3]: https://konvajs.org/docs/index.html?utm_source=chatgpt.com "Getting Started with Konva — HTML5 Canvas 2D Framework"
[4]: https://fabricjs.com/?utm_source=chatgpt.com "Fabric.js Javascript Library"
[5]: https://github.com/handsontable/hyperformula?utm_source=chatgpt.com "HyperFormula is an open-source headless spreadsheet for ..."
[6]: https://github.com/handsontable/hyperformula/discussions/1325?utm_source=chatgpt.com "Serializing HyperFormula as Immutable Cell References"
[7]: https://docs.yjs.dev/?utm_source=chatgpt.com "Yjs Docs: Introduction"
[8]: https://automerge.org/docs/hello/?utm_source=chatgpt.com "Welcome to Automerge"
[9]: https://v2.tauri.app/start/?utm_source=chatgpt.com "What is Tauri?"
[10]: https://github.com/pixijs/pixijs/blob/dev/LICENSE?utm_source=chatgpt.com "pixijs/LICENSE at dev"
[11]: https://github.com/yjs/y-protocols/blob/master/LICENSE?utm_source=chatgpt.com "y-protocols/LICENSE at master"
[12]: https://github.com/pyodide/pyodide/blob/main/LICENSE?utm_source=chatgpt.com "pyodide/LICENSE at main"
[13]: https://pyodide.com/?utm_source=chatgpt.com "Pyodide – Run Python in Browser with WebAssembly"

# Google Gemini

Dies ist eine faszinierende architektonische Herausforderung. Das **ShapeSheet-Konzept** ist das "Gehirn" von Visio, und dessen Fehlen ist exakt der Grund, warum moderne Tools wie Lucidchart oder tldraw für technische Logik oft versagen. Sie sind "Picture-first", während Siqiy "Logic-first" sein will.

Hier ist die kritische Analyse deines Tech-Stacks aus der Sicht eines Softwarearchitekten.

---

## 1. Rendering-Engine: Der Hybrid-Ansatz (PixiJS + SVG)

**Urteil: Riskant und unnötig komplex.**

Ein Hybrid-Ansatz aus WebGL (Pixi) und SVG-Overlay für Text/Connectors klingt theoretisch nach "Best of both worlds", ist aber in der Praxis ein Albtraum für die Synchronisation:

* **Koordinaten-Drift:** Bei schnellem Zoomen und Panning müssen zwei unterschiedliche DOM-Layer (Canvas und SVG) absolut frame-perfekt synchronisiert werden. Jede Millisekunde Versatz führt zu einem "Schwimmen" des Textes auf der Shape.
* **Text-Rendering:** PixiJS (WebGL) ist exzellent für Sprites, aber schwach bei dynamischem Text-Layout (Umbruch, Text-Padding in Shapes). SVG hingegen ist großartig für Text, skaliert aber bei 2000+ Objekten im DOM schlecht.
* **Empfehlung:** Setze auf einen **Single-Layer Canvas-Ansatz**.
* **Alternative:** **Fabric.js (v6+)** oder **Konva.js**. Diese bieten integriertes Hit-Testing (welche Shape wurde geklickt?), was PixiJS fehlt.
* **High-End Wahl:** **Skia via CanvasKit (WebAssembly)**. Das ist das Schwergewicht, das auch Figma nutzt. Es bietet die höchste Präzision für Typografie und Geometrie bei maximaler Performance.



---

## 2. Formel-Engine: HyperFormula

**Urteil: Gut für den Start, aber architektonisch limitiert.**

HyperFormula ist für tabellarische Daten (A1, B2) optimiert. Ein ShapeSheet ist jedoch ein **graphbasierter Dependency-Graph**.

* **Skalierung:** 2000 Shapes × 50 Cells = 100.000 Dependency-Knoten. HyperFormula bewältigt das, aber die API ist darauf ausgelegt, ein "Sheet" zu sein. In Siqiy referenzieren Shapes sich über IDs (`Shape5!Width`).
* **Empfehlung:** Nutze **Signals** (z.B. `@preact/signals` oder `nanostores`). Signals sind im Grunde atomare ShapeSheet-Zellen. Sie sind von Natur aus reaktiv, extrem performant und erlauben es, Abhängigkeiten über Objektgrenzen hinweg aufzulösen, ohne eine riesige virtuelle Excel-Tabelle im Speicher zu halten.

---

## 3. Kollaboration & State: Yjs vs. Automerge

**Urteil: Yjs ist die richtige Wahl, aber die Integration ist die "Todeszone".**

Das größte Risiko ist der **Konflikt zwischen CRDT und Formel-Engine**:

1. Nutzer A ändert `Width` von Shape 1.
2. Nutzer B hat eine Formel `Shape2!Width = Shape1!Width * 2`.
3. Wenn das CRDT den Wert von Nutzer A synchronisiert, muss die lokale Formel-Engine von Nutzer B sofort triggern.

* **Lösung:** Der "Single Source of Truth" muss das CRDT (Yjs) sein. Die Formel-Engine darf nur ein *Derivat* oder ein *Observer* des CRDT-States sein. Speichere nur die Formel-Strings im CRDT, berechne die Ergebnisse lokal.
* **State-Management:** **Zustand** ist okay für UI-State (Sidebars, Menüs), aber für den Dokument-State ist es redundant, wenn du Yjs nutzt. Nutze Yjs direkt als State-Provider.

---

## 4. Connector-Routing: draw.io Erbe

**Urteil: Realistisch, aber wartungsintensiv.**

Der draw.io Code ist "battle-tested", aber er ist in altem ES5/prozeduralem Stil geschrieben. Ihn in eine moderne TS-Architektur zu pressen, erfordert einen massiven Refactoring-Aufwand.

* **Alternative:** Schau dir **`routing-graph`** oder die Algorithmen von **`webcola`** (Constraint-based Layout) an.
* **Wichtig:** Ein "Smart Connector" in Visio-Qualität benötigt ein **Spatial Indexing** (wie RBush), um schnell zu berechnen, welche Shapes den Pfad blockieren, ohne jedes Mal alle 2000 Shapes zu prüfen.

---

## 5. Desktop & Framework: Tauri + React

**Urteil: Solide Wahl.**

Tauri ist für ein Grafik-Tool perfekt, da es den Overhead von Electron vermeidet.

* **Framework-Check:** React ist der Industriestandard für das Plugin-System (einfacher für Community-Entwickler). Aber: Vermeide es, die Shapes selbst als React-Komponenten zu rendern (wie React Flow). Das killt die Performance bei 500+ Shapes. React sollte nur das "Chrom" (UI drumherum) steuern.

---

## 6. Gesamtrisiko-Analyse

| Risiko | Schweregrad | Grund |
| --- | --- | --- |
| **Formel-Loops** | Hoch | Zirkelbezüge (`A=B; B=A`) in einem verteilten System (CRDT) können zum Absturz führen. |
| **Undo/Redo** | Mittel | Die Kombination aus Formel-Berechnungen und CRDT macht eine saubere Undo-Historie komplex. |
| **Dateiformat** | Gering | JSON ist für Diffs okay, solange die Keys sortiert gespeichert werden. |

---

## Der optimierte "Siqiy-Stack" (Empfehlung)

Dies wäre meine Architektur-Empfehlung für maximale Langlebigkeit und Performance:

* **Rendering:** **CanvasKit (Skia/Wasm)**. Wenn das zu schwerfällig ist: **Konva.js** (wegen des exzellenten Event-Handlings und der Layer-Struktur). Kein Hybrid-SVG.
* **Logic-Core:** **Signals** (für die Zellen-Reaktivität) kombiniert mit einer eigenen kleinen Parser-Engine (basiert auf `antlr4` oder `chevrotain`) für die Visio-ähnliche Syntax.
* **Persistence/Sync:** **Yjs** (CRDT) als primärer Datenspeicher.
* **Routing:** Custom Implementierung basierend auf **Obstacle-avoiding A*** unter Nutzung von **RBush** für räumliche Abfragen.
* **Scripting:** **TypeScript** (Host) + **Pyodide** (Plugin-Option für Ingenieure).
* **UI:** **React + shadcn/ui** (nur für die Shell/Editor-Panels).

### Nächster Schritt zur Validierung

Möchtest du, dass ich ein konkretes **Datenmodell für ein Shape-Objekt** skizziere, das sowohl CRDT-kompatibel als auch für die Formel-Engine optimiert ist?

# DeepSeek

## Analyse des Tech-Stacks für Siqiy

### 1. Rendering-Engine: PixiJS + SVG-Overlay

**Bewertung:**
- **PixiJS** ist eine ausgezeichnete Wahl für hohe Shape-Zahlen (2000+). WebGL-basiert, schnell, gut getestet.  
- **SVG-Overlay** für Connectors und Text:  
  - **Vorteile**: Text in SVG ist hochwertig (Schriftarten, Umbruch, Stile) und einfach zu implementieren.  
  - **Nachteile**:  
    - Koordination zwischen WebGL- und SVG-Layer ist komplex (Positionen, Transformationen, Hit-Testing).  
    - Bei vielen Text-Elementen (z.B. 2000 Beschriftungen) kann das SVG-DOM träge werden.  
    - Änderungen an Shape-Positionen (durch Formeln oder Nutzer) erfordern ständige Aktualisierung der SVG-Elemente.  
- **Alternative**: **Pixi.Text** (rendert Text in Canvas mittels Canvas2D-API und lädt ihn als Textur). Das eliminiert den separaten Layer, skaliert besser (weil nur ein Canvas), und Pixi übernimmt das Rendering. Die Textqualität ist für Diagramme völlig ausreichend (Schärfe durch Subpixel-Rendering, Zoomen durch erneutes Rendern).  
- **Empfehlung**: Für MVP nur Pixi (inkl. Pixi.Text) verwenden. Falls später komplexe Text-Interaktionen (Inline-Editing, Rich Text) nötig werden, kann man ein HTML-Overlay nur für die gerade bearbeiteten Textfelder nutzen (ähnlich wie bei Excalidraw). Das reduziert die Komplexität und Performance-Risiken.

**Risiko**: Pixi.Text hat Einschränkungen bei sehr vielen verschiedenen Schriftarten und -größen (jede Kombination erzeugt eine neue Textur). Das ist für Diagramme mit konsistenten Stilen aber meist unkritisch.

---

### 2. Formel-Engine: HyperFormula

**Bewertung:**
- **HyperFormula** ist technisch geeignet (reaktiver Dependency-Graph, viele Funktionen, Multi-Sheet).  
- **Ausschlusskriterium**: Lizenz (GPL-3.0). Siqiy sollte unter einer permissiven Lizenz (MIT/Apache 2.0) stehen, um Adoption und kommerzielle Einbettung zu fördern. Eine GPL-Kernkomponente würde das gesamte Projekt GPL-lizenzieren – ein schwerwiegendes Hindernis für viele potenzielle Nutzer und Contributor.  
- **Alternativen**:
  1. **Eigenentwicklung einer Formel-Engine**:  
     - **Formelparser**: `math.js` (MIT) oder `expr-eval` (MIT) bieten sichere Auswertung.  
     - **Reaktivität**: Leichter Dependency-Graph (z.B. mit `MobX` oder einfachem `Map<Cell, Set<Cell>>`). Bei Änderung einer Zelle werden abhängige Zellen neu berechnet (Topologische Sortierung).  
     - **Vorteile**: Volle Kontrolle, Integration mit Einheitensystem, Typ-System, Shape-Metadaten.  
     - **Nachteile**: Entwicklungsaufwand, aber das Herzstück des Produkts – es lohnt sich, hier eigene Expertise aufzubauen.  
  2. **`formulajs` + eigenes Dependency-Tracking**: `formulajs` (MIT) implementiert Excel-Funktionen, aber ohne Reaktivität. Man müsste die Abhängigkeiten selbst verwalten.  
  3. **Svelte Stores / Solid Signals**: Könnten als Reaktivitätssystem dienen, aber Formelauswertung müsste integriert werden.  

- **Empfehlung**: Eigenbau einer schlanken Formel-Engine. Start mit einem einfachen DAG und einer begrenzten Funktionsmenge (Grundrechenarten, If, Referenzen). Schrittweise Erweiterung um Excel-ähnliche Funktionen. So bleibt das Projekt unabhängig und lizenzkonform.

---

### 3. Kollaboration & State: Yjs, Zustand

**Bewertung:**
- **Yjs** ist eine hervorragende Wahl für CRDT-basierte Kollaboration. MIT-lizenziert, gut dokumentiert, aktive Community. Die Integration mit React ist über `y-react` oder manuelle Hooks möglich.  
- **Herausforderung**: Zusammenspiel mit dem reaktiven Formelgraphen.  
  - Yjs propagiert Änderungen auf Datenebene (z.B. eine Zellen-Formel wird geändert).  
  - Die Formel-Engine muss auf diese Änderungen reagieren und lokal neu berechnen.  
  - Gleichzeitige Änderungen an abhängigen Zellen durch verschiedene Nutzer können temporäre Inkonsistenzen erzeugen, die sich nach CRDT-Konvergenz auflösen. Die Formel-Engine sollte dies tolerieren (z.B. durch erneute Berechnung bei Aktualisierung).  
  - Eine Möglichkeit: Die Formel-Engine arbeitet auf einem "Shadow State", der durch Yjs-Updates aktualisiert wird. Berechnungen laufen immer auf dem konsistenten Zustand nach Anwendung aller CRDT-Operations.  
- **Zustand** für lokalen UI-State ist sinnvoll und harmoniert mit Yjs (z.B. Selektion, UI-Zustände bleiben lokal).  
- **Empfehlung**: Yjs + Zustand beibehalten. Frühzeitig Prototypen für die Integration mit der Formel-Engine bauen.

---

### 4. Connector-Routing: draw.io Routing-Algorithmen (MIT)

**Bewertung:**
- **draw.io** (mxGraph) enthält einen erprobten orthogonalen Router. Der Code ist unter Apache 2.0 lizenziert (mxGraph-Core).  
- **Herausforderungen**:  
  - Den Routing-Algorithmus aus dem umfangreichen mxGraph-Code zu extrahieren ist aufwändig und erfordert tiefes Verständnis.  
  - Der Router ist eng mit dem mxGraph-Datenmodell verwoben; eine Abstraktion für Siqiy müsste gebaut werden.  
  - Eigene Port-Typen und Ankerpunkte müssen unterstützt werden.  
- **Alternative**:  
  - **Eigene Routing-Implementierung**: Ein A*-Algorithmus auf einem Raster (z.B. 10x10 Raster) mit Hindernisvermeidung ist machbar, aber orthogonales Routing mit minimalen Knicken ist komplex.  
  - **Abstufung**: Für MVP zunächst nur direkte Linien (gerade oder benutzergesteuerte Punkte) anbieten. Das Routing kann später als Plugin oder optionales Feature nachgeliefert werden.  
  - **Bibliothek**: `pathfinding` (Browser) für A* auf Raster, kombiniert mit einem vereinfachten Modell (Hindernisse als belegte Zellen). Die Qualität ist für viele Diagramme akzeptabel, aber nicht perfekt (z.B. enge Kurven).  
- **Empfehlung**: Für die erste Version auf einfache Linien beschränken (Nutzer setzt Verbindungspunkte manuell). Das Routing als eigenständiges Modul später priorisieren. Die draw.io-Router-Integration bleibt eine Option für die Zukunft, wenn die Architektur stabil ist.

---

### 5. Desktop & Framework: Tauri, React

**Bewertung:**
- **Tauri** ist ideal: kleiner Footprint, sicher, Rust-Backend für Dateisystemzugriffe (falls benötigt). Web-first-Ansatz erlaubt einfaches Deployment als PWA und Desktop-App.  
- **React** für UI-Komponenten:  
  - Vorteile: Großes Ökosystem (shadcn/ui, Radix, etc.), einfache Entwicklerrekrutierung.  
  - Potenzielle Performance-Probleme: Da die Shapes selbst in Pixi gerendert werden, sind React-Updates nur für UI-Elemente (Sidebars, Dialoge) relevant – das ist unkritisch.  
  - Alternative **Solid.js** oder **Svelte** wären performanter, aber das Ökosystem für fertige UI-Komponenten ist kleiner. Da die UI nicht das Rendering der Shapes betrifft, ist React akzeptabel.  
- **Empfehlung**: React + Tauri beibehalten. Für zukünftige Performance-Optimierungen kann man React-Komponenten gezielt mit `memo` und `useMemo` optimieren.

---

### 6. Weitere Komponenten

- **Pyodide** für Python-Scripting: Ausgezeichnete Wahl (WebAssembly, MIT-lizenziert).  
- **Dateiformat**: JSON-basiert, von Yjs unterstützt. Für Git-Kompatibilität sollte das Format pretty-printed und zeilenweise organisiert sein (z.B. eine Shape pro Zeile oder JSON Lines).  
- **Plugin-System**: Analog zu VS Code – mit `import()`-Lazy-Loading und isolierten Sandboxes. Das kann später entwickelt werden.

---

### 7. Gesamtrisiko und schwer rückgängig zu machende Entscheidungen

- **Größtes Risiko**: Integration von CRDT (Yjs) mit reaktivem Formelgraphen. Fehler hier können zu nicht-deterministischem Verhalten führen.  
- **Schwer rückgängig zu machen**:  
  - Wahl des Rendering-Ansatzes (Pixi + HTML-Overlay vs. reines Pixi). Ein Umstieg später ist sehr aufwändig.  
  - Datenmodell (wie Shapes in Yjs abgebildet werden).  
- **Wartungslast**:  
  - Eigene Formel-Engine erfordert kontinuierliche Pflege (neue Funktionen, Bugfixes).  
  - Routing-Modul (egal ob eigen oder adaptiert) ist komplex und fehleranfällig.

---

### 8. Empfehlung – optimierter Tech-Stack für Siqiy

| Komponente           | Vorgeschlagen                     | Optimierte Wahl                     | Begründung                                                                                     |
|----------------------|-----------------------------------|--------------------------------------|-------------------------------------------------------------------------------------------------|
| UI                   | React + TypeScript + shadcn/ui   | **Beibehalten**                      | Bewährtes Ökosystem, UI bleibt getrennt vom Canvas.                                            |
| Rendering            | PixiJS + SVG-Overlay              | **PixiJS (inkl. Pixi.Text)**         | Reduziert Komplexität, vermeidet Synchronisationsprobleme; Text-Qualität ausreichend.         |
| Formel-Engine        | HyperFormula (GPL)                 | **Eigenentwicklung** (DAG + Parser)  | Lizenzkonflikt vermeiden, volle Kontrolle über Einheiten/Typen; Kernkompetenz.                 |
| Routing              | draw.io-Router (Apache 2.0)        | **Einfache Linien (MVP)** + später eigenes Routing | Reduziert initialen Aufwand; Routing kann später als Plugin folgen.                             |
| Kollaboration        | Yjs + Zustand                      | **Beibehalten**                      | Yjs ist etabliert, Zustand für UI-State sinnvoll.                                              |
| Python-Scripting     | Pyodide                            | **Beibehalten**                      | Ideal für Ingenieure, Pyodide reif und gut integrierbar.                                       |
| Desktop              | Tauri                              | **Beibehalten**                      | Web-first, leichtgewichtig.                                                                     |
| Dateiformat          | JSON/XML                           | **JSON (Yjs-kompatibel, pretty)**    | Git-freundlich, einfache Diffbarkeit.                                                           |

**Architektur-Skizze**:
- **Yjs** speichert den Dokumentenzustand (Shapes, Seiten, Stencils) als `Y.Map` und `Y.Array`.  
- Eine **reaktive Schicht** (eigene Formel-Engine) lauscht auf Yjs-Änderungen und aktualisiert berechnete Werte (z.B. Shape-Geometrie).  
- **Pixi** rendert Shapes basierend auf den berechneten Werten.  
- **React** verwaltet UI und interagiert über Hooks mit Yjs und der Formel-Engine.

**Nächste Schritte**:
1. Prototyp: Einfache rechteckige Shape mit Position, Größe, Farbe, die per Formel gesteuert wird.  
2. Integration von Yjs für zwei gleichzeitige Clients.  
3. Erweiterung auf mehrere Shapes und einfache Abhängigkeiten zwischen Shapes.  

Mit diesem Stack bleiben die Risiken kontrollierbar, und das Projekt kann schrittweise wachsen.

# Le Chat

Hier ist eine strukturierte, kritische Analyse des vorgeschlagenen Tech-Stacks für **Siqiy**, basierend auf deinen Anforderungen und den spezifischen Herausforderungen des Projekts. Ich gehe auf jede Komponente ein, identifiziere potenzielle Risiken und schlage Alternativen vor, wo sinnvoll.

---

## 1. **Rendering-Engine: PixiJS + SVG-Overlay**

### **Stärken**
- **PixiJS** ist eine bewährte, performante WebGL-basierte Rendering-Engine, die für komplexe, interaktive Grafiken optimiert ist. Sie skaliert gut bei vielen Shapes und unterstützt HiDPI-Displays.
- **SVG-Overlay** für Connectors und Text ermöglicht präzises, vektorbasiertes Rendering, was für Diagramme und Textdarstellung ideal ist.

### **Schwächen & Risiken**
- **Hybrid-Ansatz (WebGL + SVG):**
  - **Komplexität:** Die Synchronisation zwischen WebGL (PixiJS) und SVG kann zu Performance-Problemen führen, insbesondere bei häufigen Updates oder Zooming.
  - **Event-Handling:** Input-Events (z.B. Klicks, Drags) müssen zwischen beiden Layern koordiniert werden, was zu Bugs führen kann.
  - **Text-Rendering:** SVG-Text ist zwar präzise, aber bei vielen Text-Elementen kann die Performance leiden. PixiJS-Text ist weniger präzise, aber performanter.
- **Zoom-Levels:** Bei extremem Zooming (z.B. 1000%) kann es zu Artefakten oder Performance-Einbrüchen kommen, besonders wenn SVG und WebGL nicht perfekt synchronisiert sind.
- **Alternativen:**
  - **Rein Canvas-basiert (Konva.js, Fabric.js):** Einfacher zu handhaben, aber weniger performant bei sehr vielen Shapes.
  - **Rein SVG-basiert:** Ideal für präzise Darstellung, aber Performance-Probleme bei >2000 Shapes.
  - **Three.js:** Für 3D nicht nötig, aber für 2D überkill und komplexer als PixiJS.
  - **ZIM JS:** Basierend auf PixiJS, aber mit höherem Abstraktionslevel für UI-Elemente.

### **Empfehlung**
- **PixiJS + SVG-Overlay beibehalten**, aber:
  - **Text-Rendering:** PixiJS-Bitmap-Text für Performance, SVG-Text nur für statische Labels.
  - **Event-Handling:** Ein zentrales Koordinationslayer implementieren, um Input-Events zwischen PixiJS und SVG zu synchronisieren.
  - **Zoom-Optimierung:** Dynamisches Umschalten zwischen SVG und WebGL je nach Zoom-Level (z.B. SVG für <500%, WebGL für >500%).

---

## 2. **Formel-Engine: HyperFormula**

### **Stärken**
- **Reaktiver Dependency-Graph:** HyperFormula ist für Excel-ähnliche Abhängigkeiten optimiert und unterstützt Zirkelbezüge.
- **Open Source (MIT):** Lizenzkompatibel.
- **Skalierbarkeit:** Für Tabellen mit tausenden Zellen getestet.

### **Schwächen & Risiken**
- **Primär für Tabellen:** HyperFormula ist auf tabellarische Daten optimiert, nicht auf hierarchische Shape-Graphen.
- **Overhead:** Bei 2000+ Shapes mit je 50+ Cells könnte der Overhead für die Verwaltung der Abhängigkeiten hoch sein.
- **Integration:** Anpassung an Shape-spezifische Anforderungen (z.B. geometrische Abhängigkeiten) könnte aufwändig sein.
- **Alternativen:**
  - **MobX:** Reaktives State-Management, aber nicht für komplexe Formel-Abhängigkeiten optimiert.
  - **Custom DAG (Directed Acyclic Graph):** Eigenimplementierung mit Topological Sorting für Shape-Abhängigkeiten.
  - **Signals (z.B. Preact Signals, Solid.js):** Leichtgewichtig, aber weniger ausgereift für komplexe Abhängigkeiten.
  - **ExcelJS + Custom Logic:** Falls Excel-ähnliche Formeln gewünscht sind, aber weniger flexibel.

### **Empfehlung**
- **HyperFormula testen**, aber:
  - **Benchmarking:** Performance bei 2000+ Shapes mit 50+ Cells pro Shape testen.
  - **Fallback:** Falls HyperFormula zu schwerfällig ist, einen **custom DAG** mit Topological Sorting implementieren (z.B. mit [dagre](https://github.com/dagrejs/dagre)).
  - **Signals für UI-Reaktivität:** Kombiniert mit Yjs für Kollaboration.

---

## 3. **Kollaboration & State: Yjs + Zustand**

### **Stärken**
- **Yjs (CRDT):** Bewährt für Echtzeit-Kollaboration, offline-first, konfliktfrei.
- **Zustand:** Leichtgewichtiger State-Manager für lokalen UI-State.

### **Schwächen & Risiken**
- **Yjs + reaktiver Formel-Graph:**
  - **Konflikte:** Wenn zwei Nutzer gleichzeitig Cells bearbeiten, die sich gegenseitig referenzieren, kann es zu inkonsistenten Zuständen kommen.
  - **Performance:** Yjs ist für Text/Kollaboration optimiert, nicht für komplexe Abhängigkeitsgraphen.
- **Zustand vs. Yjs:**
  - **Redundanz:** Zustand und Yjs könnten sich überschneiden, wenn UI-State und Kollaborations-State nicht klar getrennt sind.
- **Alternativen:**
  - **Automerge:** Besser für strukturierte Daten, aber weniger etabliert als Yjs.
  - **Redux + Yjs:** Klare Trennung zwischen UI-State (Redux) und Kollaborations-State (Yjs).
  - **Custom CRDT für Formeln:** Falls Yjs zu limitierend ist.

### **Empfehlung**
- **Yjs beibehalten**, aber:
  - **Formel-Graph isolieren:** Nur die "rohen" Shape-Daten (Position, Properties) über Yjs synchronisieren, nicht die Formel-Ergebnisse.
  - **Lokale Reaktivität:** Formel-Ergebnisse lokal mit Signals oder MobX berechnen und nur bei Änderungen an Yjs senden.
  - **Konfliktlösung:** Custom Logic für Zirkelbezüge und inkonsistente Abhängigkeiten.

---

## 4. **Connector-Routing: draw.io-Algorithmen**

### **Stärken**
- **Bewährt:** draw.io hat robuste orthogonale Routing-Algorithmen.
- **MIT-Lizenz:** Kompatibel mit OSS.

### **Schwächen & Risiken**
- **Adaption:**
  - **Wartbarkeit:** draw.io's Code ist auf JavaScript/HTML5 optimiert und könnte schwer in eine React/PixiJS-Architektur zu integrieren sein.
  - **Ports:** Orthogonales Routing mit festen Ankerpunkten (Ports) erfordert präzise Geometrie-Berechnungen.
- **Alternativen:**
  - **[jsplumb](https://jsplumbtoolkit.com/):** Dedizierte Connector-Bibliothek, aber kommerziell.
  - **[jointjs](https://www.jointjs.com/):** Open-Source-Alternative mit Routing-Funktionen.
  - **Custom Implementierung:** Basierend auf [A*](https://en.wikipedia.org/wiki/A*_search_algorithm) oder [Visibility Graphs](https://en.wikipedia.org/wiki/Visibility_graph).

### **Empfehlung**
- **draw.io-Algorithmen adaptieren**, aber:
  - **Isolierte Komponente:** Als eigenständiges Modul implementieren, das nur die Routing-Logik kapselt.
  - **Benchmarking:** Performance bei 2000+ Connectors testen.
  - **Fallback:** Falls zu komplex, **jointjs** oder eine **Custom-A*-Implementierung** verwenden.

---

## 5. **Desktop & Framework: Tauri + React**

### **Stärken**
- **Tauri:**
  - Leichtgewichtig (Rust-Basis), sicherer als Electron, gute Integration mit Web-Technologien.
  - Open Source (MIT/Apache 2.0).
- **React:**
  - Große Community, gute Tooling-Unterstützung (z.B. DevTools), geeignet für komplexe UIs.

### **Schwächen & Risiken**
- **Tauri:**
  - **Reife:** Weniger etabliert als Electron, kleinere Community.
  - **Native APIs:** Nicht alle Plattform-APIs sind verfügbar.
- **React:**
  - **Performance:** Bei sehr vielen interaktiven Elementen (z.B. 2000+ Shapes) könnte die Virtual-DOM-Diffing-Logik zum Flaschenhals werden.
- **Alternativen:**
  - **Electron:** Stabiler, aber schwerer und weniger sicher.
  - **Svelte/Solid.js:** Performanter als React, aber kleinere Community.
  - **Vue:** Guter Mittelweg zwischen React und Svelte.

### **Empfehlung**
- **Tauri + React beibehalten**, aber:
  - **Performance-Optimierung:** React mit `react-memo` und `useMemo` optimieren.
  - **Fallback:** Falls React zu langsam ist, **Solid.js** oder **Svelte** testen.
  - **Tauri-Monitoring:** Regelmäßig Updates prüfen, da das Projekt schnell wächst.

---

## 6. **Gesamtrisiko & Wartungslast**

### **Größte Risiken**
1. **Formel-Engine:**
   - HyperFormula könnte zu schwerfällig für Shape-Graphen sein.
   - Custom DAG wäre wartungsintensiv.
2. **Rendering-Hybrid-Ansatz:**
   - Synchronisation zwischen PixiJS und SVG könnte zu Bugs führen.
3. **Kollaboration + Formeln:**
   - Yjs ist nicht für komplexe Abhängigkeiten optimiert.
4. **Routing:**
   - draw.io-Code könnte schwer zu warten sein.

### **Schwer rückgängig zu machende Entscheidungen**
- **Formel-Engine:** Wechsel später sehr aufwändig.
- **Rendering-Architektur:** PixiJS + SVG vs. rein Canvas/SVG.
- **Kollaborations-Backend:** Yjs vs. Automerge vs. Custom.

### **Wartungslast**
- **Custom CRDT/Formel-Engine:** Hohe Komplexität.
- **Hybrid-Rendering:** Bug-anfällig.
- **draw.io-Routing:** Schwer zu erweitern.

---

## **Abschließende Empfehlung: Optimaler Tech-Stack für Siqiy**

| Komponente          | Empfehlung                                                                 | Begründung                                                                                     |
|---------------------|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| **UI-Layer**        | React + TypeScript + shadcn/ui                                             | Bewährt, gute Community, aber Performance optimieren.                                         |
| **Rendering**       | PixiJS (WebGL) + **dynamisches SVG-Text-Overlay**                          | PixiJS für Shapes, SVG nur für Text/Connectors bei Bedarf.                                     |
| **Formel-Engine**   | **Custom DAG mit Topological Sorting** (Fallback: HyperFormula)           | Flexibler für Shape-Abhängigkeiten, weniger Overhead.                                          |
| **Python-Runtime**  | Pyodide                                                                     | Bewährt für Python in WASM.                                                                   |
| **Kollaboration**   | Yjs (nur für Shape-Daten) + **Signals für lokale Reaktivität**              | Yjs für Sync, Signals für Performance.                                                         |
| **UI-State**        | **Jotai** (statt Zustand)                                                  | Atomare State-Updates, besser für komplexe Abhängigkeiten.                                      |
| **Routing**         | **Custom A*-Routing** (Fallback: draw.io-Algorithmen)                      | Flexibler und wartbarer als draw.io-Code.                                                      |
| **Desktop**         | Tauri                                                                      | Leichtgewichtig und sicher.                                                                   |
| **Dateiformat**     | **JSON mit Canonicalisierung** (für Git-Diffs)                             | Einfacher zu parsen als XML, besser für Git.                                                   |
| **Plugin-System**   | **VS Code-ähnliches Extension-API**                                       | Erweitert die Lebensdauer des Projekts.                                                        |

---

## **Alternativer Architektur-Ansatz: Phasenweise Migration**

1. **Phase 1 (MVP):**
   - Rein SVG-basiert (kein PixiJS) für einfache Shapes/Connectors.
   - Formel-Engine: MobX für Reaktivität, keine komplexen Abhängigkeiten.
   - Kollaboration: Yjs nur für Basis-Sync.
   - Routing: jointjs oder Custom A*.

2. **Phase 2 (Skalierung):**
   - PixiJS für Shapes hinzufügen, SVG nur für Text/Connectors.
   - Custom DAG für Formeln implementieren.
   - Yjs + Signals für Kollaboration optimieren.

3. **Phase 3 (Desktop/Plugins):**
   - Tauri-Integration.
   - Plugin-API entwerfen.

---
**Fazit:**
Der vorgeschlagene Stack ist solide, aber mit **kritischen Risiken bei Formel-Engine, Rendering-Hybrid-Ansatz und Kollaboration**. Mein optimierter Stack setzt auf **mehr Flexibilität (Custom DAG, dynamisches Rendering)** und **klarere Trennung der Verantwortlichkeiten** (Yjs nur für Sync, Signals für Reaktivität). Ein phasenweiser Ansatz reduziert das Risiko und ermöglicht iterative Optimierung.

---
**Frage an dich, Yacine:**
- Welche der identifizierten Risiken siehst du als am kritischsten für dein Projekt?
- Soll ich zu einer der Komponenten (z.B. Formel-Engine oder Rendering) detailliertere Code-Beispiele oder Benchmark-Ideen ausarbeiten?

# Qwen

Tech-Stack-Analyse für Siqiy: Kritische Architekturbewertung

Als Softwarearchitekt, der ähnliche Grafik-Editoren und reaktive Systeme begleitet hat, analysiere ich den vorgeschlagenen Stack ohne "Vendor-Loyalität". Meine Prämisse: **Komplexität ist der Feind von OSS-Projekten**. Jede Schicht muss ihren Mehrwert beweisen.

---

## 1. Rendering-Engine: PixiJS + SVG-Overlay

### ⚠️ Kritische Probleme des Hybrid-Ansatzes

```
WebGL (PixiJS) für Shapes
       ↓
[Transform-Sync] ← Fehlerquelle #1
       ↓
SVG-Overlay für Connectors/Text
       ↓
DOM-Layer für UI
```

**Konkrete Risiken:**
- **Event-Handling**: Hit-Testing muss über drei Layer hinweg koordiniert werden. Klick auf einen Connector? Ist er im SVG-Layer oder berührt er eine WebGL-Shape?
- **Transform-Sync**: Zoom/Pan muss exakt synchronisiert werden. Subpixel-Unterschiede führen zu "flackernden" Connectors.
- **Text-Rendering**: WebGL-Text erfordert SDF-Fonts, ist aufwändig für dynamischen Content. SVG-Text ist qualitativ besser – aber dann warum nicht alles in SVG?
- **Z-Ordering**: Welche Layer liegt "oben"? Shapes mit Connectors, die durch andere Shapes verlaufen?

### 🔍 Alternativen im Vergleich

| Engine | Max. Shapes* | Text-Qualität | Hit-Testing | Lernkurve |
|--------|-------------|---------------|-------------|-----------|
| **Konva.js** (Canvas) | ~3.000 | ★★★★☆ | Native | Flach |
| **PixiJS** (WebGL) | ~15.000+ | ★★☆☆☆ (SDF) | Custom nötig | Steil |
| **Pure SVG** | ~500 | ★★★★★ | Native | Sehr flach |
| **Fabric.js** | ~2.000 | ★★★★☆ | Native | Mittel |

*\*Richtwerte für interaktive Framerate (60fps) auf Mittelklasse-Hardware*

### ✅ Empfehlung
**Starte mit Konva.js für den MVP.** 
- Canvas 2D bietet das beste Preis-Leistungs-Verhältnis für <2.000 Shapes
- Integriertes Hit-Testing, gute Text-Unterstützung, aktive Community
- Wenn Profiling zeigt, dass Rendering zum Flaschenhals wird: Migration zu PixiJS ist möglich (beide nutzen ähnliche Konzepte)
- **Vermeide den Hybrid-Ansatz** bis du ihn wirklich brauchst – premature optimization ist hier der größte Feind

---

## 2. Formel-Engine: HyperFormula

### ⚠️ Das Grundproblem: Grid vs. Graph

HyperFormula ist exzellent für **tabellarische Daten** (Zellen in Rows/Columns). Das ShapeSheet-Modell ist jedoch ein **semantischer Objektgraph**:

```javascript
// HyperFormula-Modell (Grid)
getCell("A1") → "Width"
getCell("B1") → "=A1 * 1.5"

// ShapeSheet-Modell (Objektgraph)
Shape1.Properties.Width → "100px"
Shape2.Properties.X → "=Shape1.Properties.X + Shape1.Properties.Width"
```

**Konsequenzen:**
- Mapping von `Shape.Property` auf `Cell-Reference` erfordert komplexe Abstraktion
- HyperFormulas Dependency-Graph ist auf Grid-Topologie optimiert, nicht auf beliebige Objekt-Referenzen
- Performance: 2.000 Shapes × 50 Properties = 100.000 "Zellen" mit cross-references → ungetestetes Terrain für HyperFormula

### 🔍 Alternativen

| Ansatz | Pros | Cons |
|--------|------|------|
| **Custom DAG-Engine** | Tailored für ShapeSheet, volle Kontrolle | Entwicklungszeit, Fehleranfälligkeit |
| **MobX** | Bewährt, fine-grained reactivity | UI-fokussiert, nicht für Formel-Parsing |
| **Solid.js Signals** | Extrem granular, performant | Noch relativ jung, weniger Docs |
| **XState + Parser** | Gut für komplexe State-Logik | Overkill für reine Formelauswertung |

### ✅ Empfehlung
**Baue eine minimale, domain-spezifische Reactive Engine.**

```typescript
// Minimal viable reactive engine sketch
class ReactiveProperty {
  value: any;
  dependencies: Set<ReactiveProperty>;
  subscribers: Set<() => void>;
  
  setFormula(expr: string, context: EvaluationContext) {
    // Parse expr → AST, extract dependencies
    // Build DAG, topological sort for evaluation order
  }
  
  notify() {
    // Topological evaluation, avoid circular refs
    this.subscribers.forEach(cb => cb());
  }
}
```

- Starte mit einem simplen Pub/Sub + Topological Sort
- Erweitere nur, wenn du Excel-Features wirklich brauchst (Matrix-Operationen, komplexe Funktionen)
- **Vorteil**: Du kontrollierst die Semantik von `Shape.Property`-Referenzen von Anfang an

---

## 3. Kollaboration & State: Yjs + Zustand

### 🔥 Das unterschätzte Risiko: Semantic CRDTs

Yjs löst **Datenkonflikte** exzellent. Aber dein Use-Case hat **semantische Konflikte**:

```
User A: Bearbeitet Formel: "Width = Height * 2"
User B: Löscht Property "Height" in derselben Shape
→ Wer gewinnt? Was ist der konsistente Zustand?
```

**Yjs allein kann das nicht lösen.** Du brauchst eine **Anwendungsschicht für Konflikt-Strategien**:
- Last-write-wins für einfache Properties
- Formel-Validierung vor Anwendung (circular reference check)
- Optional: Operational Transform auf Formel-AST-Ebene (Forschungsthema!)

### Zustand vs. Yjs für UI-State

```
❌ Problem: Zwei State-Sources
Yjs: Document State (Shapes, Formeln)
Zustand: UI State (Sidebar offen, aktive Tool)

→ Sync-Overhead, schwer zu debuggen
```

### ✅ Empfehlungen
1. **Bleib bei Yjs** (reifer als Automerge, bessere TS-Support), aber:
   - Definiere früh **Conflict Resolution Policies** für Formeln
   - Implementiere eine "Formula Evaluation Queue", die Rekalkulation serialisiert
2. **Konsolidiere State**: Nutze `yjs-react` Bindings, um UI-Komponenten direkt an Yjs-Dokumente zu binden. Minimale lokale State nur für transient UI (Hover, Animationen).
3. **Teste Kollaboration früh**: Baue einen minimalen Multi-User-Test mit zwei Browser-Tabs, bevor du in Rendering investierst.

---

## 4. Connector-Routing: draw.io Code-Adaption

### ⚠️ Realitätscheck

draw.io's Routing-Code ist MIT-lizenziert – aber:
- Stark gekoppelt an draw.io's internes Modell (Geometry, Graph-Struktur)
- Dokumentation: praktisch nicht vorhanden
- Wartbarkeit: Du forkst einen Code, den du nicht vollständig verstehst

### 🔍 Bessere Alternative: elkjs

[Eclipse Layout Kernel (elkjs)](https://github.com/kieler/elkjs) ist **genau für diesen Use-Case designed**:
- Orthogonales Edge-Routing mit Port-Semantik
- Aktive Wartung, gute TypeScript-Typen
- Konfigurierbare Strategien (Layered, Tree, Radial)
- Beispiel für orthogonal routing:

```javascript
import { ELK } from 'elkjs/lib/elk.bundled.js';

const elk = new ELK();
const layout = await elk.layout({
  id: 'root',
  children: [/* nodes */],
  edges: [/* edges */],
  layoutOptions: {
    'elk.algorithm': 'layered',
    'elk.edgeRouting': 'ORTHOGONAL',
  }
});
```

### ✅ Empfehlung
**Evaluiere elkjs als Primary-Option.** 
- Wenn draw.io-Code sich als trivial extrahierbar erweist: super, nutze ihn
- Aber plane nicht darauf – elkjs ist die sicherere Wette für Wartbarkeit

---

## 5. Desktop & Framework: Tauri + React

### Tauri: Pro & Contra

| Pro | Contra |
|-----|---------|
| Kleine Bundle-Size (~3MB vs. 100MB Electron) | Junge Ökosystem, gelegentliche Rough Edges |
| Rust-Backend für Performance-kritische Tasks | Weniger Plugins/Integrationen als Electron |
| Web-first Ansatz passt perfekt zu Siqiy | Debugging von Rust+Web kann komplex sein |

**Fazit**: Tauri ist die richtige Wahl für ein OSS-Projekt mit technischer Zielgruppe.

### React vs. Alternativen

Für eine **Canvas-heavy App** ist die UI-Framework-Wahl sekundär – der Großteil der Performance liegt im Rendering-Layer. Trotzdem:

| Framework | Re-Render-Overhead | Learning Curve | Ecosystem |
|-----------|-------------------|----------------|-----------|
| **React** | Mittel (Virtual DOM) | Flach | Riesig |
| **Solid.js** | Sehr niedrig (fine-grained) | Mittel (React-ähnlich) | Wachsend |
| **Svelte** | Niedrig (compile-time) | Mittel | Solide |

**Empfehlung**: React ist völlig in Ordnung. Wenn du jedoch maximale UI-Responsiveness willst (z.B. Sidebar-Updates bei Shape-Selection), ist **Solid.js** einen Blick wert – ähnliche Syntax, bessere Performance für fine-grained Updates.

---

## 6. Gesamtrisiko-Analyse

### 🚨 Top 3 Technische Risiken

1. **CRDT + Reactive Formulas** (Kritikalität: 🔴 Hoch)
   - Kollaborative Bearbeitung von interdependenten Formeln ist ein ungelöstes Forschungsproblem
   - **Mitigation**: Definiere früh "Conflict Semantics", starte mit Single-User, erweitere schrittweise

2. **Data Model Lock-in** (Kritikalität: 🟠 Mittel-Hoch)
   - Wenn du ShapeSheet in HyperFormulas Grid-Modell presst, wird Migration später schmerzhaft
   - **Mitigation**: Abstrahiere `PropertyReference` als Interface, halte Engine austauschbar

3. **Plugin-API Design** (Kritikalität: 🟡 Mittel)
   - Schlecht designede Extension Points fragmentieren das Ökosystem
   - **Mitigation**: Orientiere dich an VS Codes API-Design: klare Versionierung, Sandbox-Execution

### 🔁 Hardest to Reverse
**Die Wahl des Shape-Property-Datenmodells.** Rendering, UI-Framework, sogar CRDT-Layer können migriert werden – aber wenn `Shape.Properties` einmal festgezurrt ist, wird jede Änderung zur Migration für alle existierenden Dokumente.

### 🛠️ OSS Maintenance Burden
- Custom Formula Engine: Hoch, wenn nicht gut abstrahiert
- Plugin-System: Hoch, wenn nicht versioniert und dokumentiert
- CRDT-Debugging: Sehr hoch (kollaborative Bugs sind notorisch schwer zu reproduzieren)

---

## 🎯 Abschließende Empfehlung: Der optimierte Stack

### ✅ Beibehalten
- **TypeScript** als Primärsprache
- **Pyodide** für Python-Skripting (exzellente Wahl)
- **Tauri** für Desktop-Wrapping (web-first ist korrekt)
- **Offenes JSON-Format** mit semantischer Struktur

### 🔄 Ersetzen / Anpassen

| Komponente | Vorgeschlagen | Empfohlen | Begründung |
|-----------|--------------|-----------|------------|
| Rendering | PixiJS + SVG | **Konva.js (Canvas)** | Simpler, good-enough Performance, weniger Sync-Overhead |
| Formula Engine | HyperFormula | **Custom Reactive DAG** | Domain-specific, vermeidet Grid-Abstraktion |
| Routing | draw.io Code | **elkjs evaluieren** | Besser dokumentiert, decoupled Architecture |
| State | Yjs + Zustand | **Yjs als Single Source** | Vermeide zwei State-Sources, nutze yjs-react Bindings |
| UI Framework | React | React (oder Solid.js) | React ist fine, Solid.js als Option für Performance |

### 🏗️ Optimaler Stack (MVP-Fokus)

```
┌─────────────────────────────────┐
│ UI Layer                        │
│ React/Solid.js + TypeScript     │
│ shadcn/ui für Komponenten       │
├─────────────────────────────────┤
│ Rendering                       │
│ Konva.js (Canvas 2D)            │
│ → PixiJS nur bei Profiling-Need│
├─────────────────────────────────┤
│ Routing                         │
│ elkjs für orthogonales Routing  │
│ Custom Port/Anchor-System       │
├─────────────────────────────────┤
│ Formula Engine                  │
│ Minimal custom reactive DAG     │
│ Topological sort + pub/sub      │
├─────────────────────────────────┤
│ Scripting                       │
│ TypeScript native               │
│ Pyodide für Python (WASM)       │
├─────────────────────────────────┤
│ Collaboration                   │
│ Yjs (CRDT) + WebSocket          │
│ + Semantic Conflict Policies    │
├─────────────────────────────────┤
│ State Management                │
│ Yjs als Single Source of Truth  │
│ Minimal local UI state          │
├─────────────────────────────────┤
│ Desktop                         │
│ Tauri (Rust backend)            │
├─────────────────────────────────┤
│ Format                          │
│ Structured JSON + JSON-Schema   │
│ Git-friendly Diffs              │
└─────────────────────────────────┘
```

### 🧪 Alternative: Phased SVG-First-Strategie

**Erwäge diesen Ansatz für extrem schnelle Iteration:**

1. **Phase 1 (Proof of Concept)**: Pure SVG + minimal reactive engine
   - <500 Shapes, aber native Accessibility, CSS-Styling, einfache Debugging
   - Validiere das ShapeSheet-Konzept ohne Rendering-Complexity

2. **Phase 2 (Performance)**: Migration zu Konva.js (Canvas)
   - Wenn Nutzerbasis wächst und Performance messbar wird

3. **Phase 3 (Scale)**: Optional PixiJS (WebGL) für 10k+ Shapes
   - Nur wenn wirklich benötigt

**Vorteil**: Du validierst die Kern-Innovation (ShapeSheet + Progressive Automation) bevor du in komplexe Rendering-Optimierung investierst.

---

## 🔑 Critical Success Factor

> **Investiere 80% deiner frühen Architektur-Zeit in das semantische Datenmodell für Shapes und Formeln.**

Rendering, Collaboration, UI-Framework – all das kann migriert werden. Aber wenn `Shape.Property.Reference` einmal in Stein gemeißelt ist, wird jede Änderung zur Breaking Change für alle Plugins und existierenden Dokumente.

**Konkreter nächster Schritt**: Baue einen **minimalen Vertical Slice**:
- Eine Shape-Type mit 3 Properties
- Eine reaktive Formel (`Width = Height * 1.5`)
- Einfache Collaboration (zwei Tabs, Yjs-Sync)
- Keine fancy Rendering-Optimierung

Validiere damit den "Progressive Automation"-Flow. Der Rest ist Implementierungsdetail.

Viel Erfolg mit Siqiy – das Konzept hat echtes Potenzial, die Lücke im Markt ist real. 🚀

# Grok

**Kritische Analyse des vorgeschlagenen Tech-Stacks für Siqiy**

Als Architekt mit Fokus auf Grafik-Engines, webbasierte Editoren und kollaborative Systeme (Stand März 2026) habe ich den Stack unvoreingenommen geprüft – inklusive aktueller GitHub-Status, Benchmarks, Issues und Vergleichen aus der Community. Der Vorschlag ist ambitioniert und greift viele gute Ideen auf (z. B. CRDT + reaktive Formeln), birgt aber **signifikante technische Risiken**, besonders beim Rendering und der Formel-Integration. Ich priorisiere hier **Präzision, Skalierbarkeit bei 2000+ Shapes, Zoom/ HiDPI-Qualität und Wartbarkeit** – genau die Anforderungen für ein Visio-Nachfolger mit ShapeSheet und Smart Connectors.

### 1. Rendering-Engine (PixiJS + SVG-Overlay)

**Schwächen des Hybrid-Ansatzes:**  
PixiJS v8.16.0 (Feb 2026, 46,7k Stars, hochaktiv) ist extrem performant bei 10.000+ Objekten via WebGL (Benchmarks zeigen Top-Platzierungen bei Sprite-Rendering). Das passt zur Skalierungsanforderung. **Aber:** Text-Rendering ist seit Jahren ein Dauerbrenner – Text wird zu Bitmap-Texturen gerendert und wird bei Zoom unscharf/pixelig (offene Diskussionen #11696, alte Issues #968, Regressions-Bugs in v8.10+ mit vielen Text-Objekten: FPS-Einbruch von 60 auf 5). HiDPI und exaktes Vector-Verhalten (wie in Visio) fehlen nativ.  

Der **SVG-Overlay** (für Connectors/Text) ist ein gängiger Hack, erzeugt aber massive Probleme:  
- Sync von Positionen/Z-Index/Hit-Testing zwischen WebGL-Canvas und DOM-SVG-Layer (Events, Selection-Handles, Ports brechen leicht).  
- Keine native Unterstützung im Pixi-README; keine Diagram-Editor-Empfehlung.  
- Wartungslast hoch (z. B. bei Zoom, Panning, live Rerouting).  

**Vergleich Alternativen:**  
- **Konva.js** (v10.2.0 Jan 2026, 14,2k Stars, aktiv bis Feb 2026): Pure HTML5 Canvas 2D, optimiert für **interaktive Diagramme** (Gruppen, Layer, Caching, Events, Tausende Shapes mit <60 FPS). Text/Paths sind präzise, Zoom/HiDPI nativ besser handhabbar, kein Hybrid-Chaos. Vorteil gegenüber WebGL: Leichter, exakter für Vector-ähnliche Präzision (Ports, orthogonal Lines). Viele CAD-/Diagramm-Tools nutzen es genau deshalb.  
- **Fabric.js**: Ähnlich gut für Editing, aber etwas schwerfälliger bei sehr vielen Objekten als Konva.  
- Reine SVG (wie altes mxGraph/draw.io): Perfekt für Präzision/Accessibility, aber bricht bei >500–1000 Shapes ein – nicht zukunftssicher.  
- Neuere WebGPU-Optionen (Pixi experimentell, Three.js WebGPU): Noch nicht reif für präzise 2D-Diagramme (Overhead, Text-Probleme bleiben).  

**Fazit:** Hybrid ist das **größte Risiko** im gesamten Stack – schwer zu debuggen, UX-kritisch (unscharfe Labels bei Zoom zerstören das „Visio-Gefühl“). PixiJS ist overkill für Diagramme und unterfordert in der Präzision.

### 2. Formel-Engine (HyperFormula)

**Eignung:** HyperFormula v3.2.0 (Feb 2026, 2,6k Stars, aktiv mit Dependency-Graph-Refactor Jan 2026) ist exzellent für Excel-ähnliche Formeln (400+ Funktionen, smart Recalc, Named Expressions, Multi-Sheet). Dependency-Graph ist performant (60 % schneller in alten Tests; Refactor für „big spreadsheets“). Man könnte pro Shape ein eigenes Sheet + Cross-References via Named Ranges simulieren.  

**Probleme:**  
- Kern-Design ist **grid-basiert** (Zellen, Ranges). ShapeSheet ist ein **arbiträrer Objekt-Graph** (Shape1!Width referenziert Shape2!LocPinX etc.). Das führt zu Workarounds und potenziell quadratischer Komplexität bei 2000 Shapes × 50 Cells (Issues zu Map-Size-Limits bei großen Workbooks).  
- Keine bekannten Nutzungen in Diagram-/Shape-Tools – nur Spreadsheet-Apps. Keine native Unterstützung für per-Shape-Sheets oder physische Einheiten.  
- Skalierung: Bei inter-Shape-Deps gut, aber Build des Graphs kann bei CRUDs teuer werden (ältere Issues zu Minuten-Ladezeiten bei komplexen Büchern).  

**Bessere Alternativen:**  
- Custom DAG + Signals (z. B. Preact Signals, MobX, oder einfacher topo-sort mit Proxy) – natürlicher für Shapes, leichter mit CRDT zu verbinden, keine Grid-Zwangsjacke.  
- HyperFormula behalten **nur** für Parsing/Evaluation (starkes Feature), aber nicht als zentralen Graph.  

**Fazit:** Brauchbar als Start, aber nicht optimal – Risiko, dass man später refaktorieren muss.

### 3. Kollaboration & State (Yjs + Zustand)

**Stärken:** Yjs v14.0-rc.1 (Feb 2026, 21,4k Stars, sehr aktiv) ist der De-facto-Standard für Diagram/Whiteboard-Editoren (ourboard, Synergy Codes für komplexe Graphs/Diagramme). Schneller und praxiserprobt als Automerge (besser bei Real-Time, kleiner, GC-optimiert; Automerge gewinnt nur bei full History/Time-Travel). observeDeep + Bindings (valtio-yjs, SyncedStore) erlauben nahtlose Integration externer Reaktivität.  

**Risiken mit Formel-Graph:** Wenn eine CRDT-Änderung eine Formel triggert, die wiederum andere Shapes ändert → mögliche Loops oder Merge-Konflikte auf „computed“-Ebene (nicht nur Daten). Muss von Anfang an mit debounce/isolierter Recalc-Schicht gelöst werden – machbar, aber nicht trivial (keine native Unterstützung).  

Zustand ist leichtgewichtig und Yjs-kompatibel (via custom Stores). Bessere Optionen: Valtio oder SyncedStore für direktere CRDT-Reaktivität.  

**Fazit:** Yjs bleibt top – eine der stärksten Komponenten. Kein Wechsel nötig.

### 4. Connector-Routing (draw.io-Algorithmen)

**Realismus:** Ja, absolut machbar und wartbar. draw.io (jgraph/drawio) ist MIT-lizenziert, Routing-Code (orthogonal, Shape-Avoidance, Ports) ist bewährt und wird aktiv weiterentwickelt (Issues zu Snapping/Grid 2025/2026). Viele Projekte haben Teile extrahiert/portiert. Keine dedizierte „offizielle“ Library, aber der Code ist sauber genug für eine eigene Routing-Engine (A* auf Grid + Visibility-Verbesserungen).  

In Canvas-Architektur (siehe unten) einfach zu integrieren – live Rerouting bei Bewegung ist genau das, was draw.io schon kann. Aufwand: Mittel (TS-Port + Anpassung an Ports/ShapeSheet).  

**Alternativen:** ELK.js (gut für Graphs, aber weniger orthogonal), custom A*/Visibility Graph. draw.io-Code bleibt die pragmatischste Basis.  

**Fazit:** Gute Wahl, bleibt empfehlenswert.

### 5. Desktop & Framework (Tauri + React + Zustand)

**Tauri:** Klarer Gewinner 2026. Bundle 2–10 MB vs. Electron 150+ MB, RAM 30–50 MB vs. 150–300+ MB, Startup <1 s. Sicherer (kein voller Chromium-Bundle), web-first perfekt. Alle Vergleiche (2025/2026) bestätigen: Tauri ist die moderne Wahl für Diagram-Apps.  

**React:** Funktioniert super mit shadcn/ui und Tauri. Aber Svelte (oder Solid) wäre performanter/leichter (kleinerer Bundle, reaktiver native). Für komplexe UI (ShapeSheet-Editor, Inspector) ist React jedoch unschlagbar im Ökosystem. Kein Showstopper.  

**Fazit:** Tauri behalten, React optional zu Svelte migrieren für etwas mehr Speed.

### 6. Gesamtrisiko & schwer umkehrbare Entscheidungen

- **Größtes Risiko:** Rendering-Hybrid (Text/Zoom/Sync-Bugs) – trifft direkt die Kern-UX und ist am teuersten zu fixen.  
- **Schwerste Rücknahme:** Rendering-Engine (alles drumherum baut darauf auf: Ports, Handles, Events, Routing).  
- **Wartungslast bei OSS:** Custom Routing + Formel-CRDT-Integration + Plugin-API. HyperFormula und Yjs sind wartungsarm; Pixi-Hybrid wird zur Dauerbaustelle.  

**Abschließende Empfehlung – Optimaler Stack für Siqiy**

Ich würde **70 % des Stacks beibehalten**, aber die kritischen Teile ersetzen, um Risiken massiv zu senken und die Progressive-Automation-Philosophie besser zu treffen.

**Optimaler Stack (MVP-tauglich, skalierbar, wartbar):**

- **UI-Layer:** Svelte (oder React) + shadcn/ui – leichter als React, perfekter für Tauri.  
- **Rendering:** **Konva.js** (pure Canvas 2D) statt PixiJS + SVG-Overlay.  
  *Begründung:* Präzise Vector-Qualität, exzellente Interaktivität (Ports, Groups, Events), Tausende Shapes bei 60 FPS, kein Hybrid-Chaos, Zoom/HiDPI nativ stark. Perfekt für Visio-ähnliche Shapes, Text und live Rerouting. (PixiJS nur, wenn später >5000 Shapes zwingend WebGL brauchen.)  
- **Routing-Engine:** draw.io orthogonal Algorithmen (adaptieren) – bleibt, ist goldwert.  
- **Formel-Engine:** HyperFormula (für Parsing + Eval) + **custom per-Shape DAG + Signals** (z. B. mit valtio oder Preact-Signals).  
  *Begründung:* Natürliche Shape-Referenzen, keine Grid-Zwangsjacke, einfacher CRDT-Abgleich. HyperFormula als starke Bibliothek behalten, aber nicht als zentralen Store.  
- **Python-Scripting:** Pyodide – unverändert.  
- **Kollaboration & State:** Yjs (mit valtio-yjs oder SyncedStore) + WebRTC/WebSocket – unverändert, top. Lokaler State: Zustand/Valtio.  
- **Desktop:** Tauri – unverändert, perfekt.  
- **Dateiformat:** JSON-basiert + CRDT-serialisierbar (Git-Diffs super).  

**Alternative Architektur-Variante (für Phase 1 MVP):**  
Starte **rein mit Konva.js** (oder sogar temporär SVG für maximale Präzision/Accessibility bis ~500 Shapes). Das reduziert Komplexität enorm, ermöglicht schnelles ShapeSheet-Prototyping und lässt sich später auf WebGL (Pixi pure) upgraden, falls nötig. Kein Hybrid von Anfang an – das spart Monate Debugging.

Dieser Stack ist **robuster, präziser und näher am Visio-Geist** (lebendige, intelligente Shapes ohne Kompromisse bei Text/Zoom). Das größte Risiko ist eliminiert, die Progressive Automation bleibt voll erhalten. Mit Konva + Yjs + HyperFormula-Wrapper bist du in 6–9 Monaten bei einem überlegenen MVP – und die Community wird es lieben, weil es einfach funktioniert.  

Falls du konkrete PoC-Code-Snippets oder eine detaillierte Migrations-Roadmap brauchst: Sag Bescheid. Das wird ein starkes Projekt.

# z.AI

Hier ist eine kritische Analyse des vorgeschlagenen Tech-Stacks für Siqiy aus der Sicht eines erfahrenen Softwarearchitekten.

---

Review-Prompt: Tech-Stack-Analyse für Siqiy

## 1. Rendering-Engine (PixiJS + SVG-Overlay)

**Analyse:**
Der vorgeschlagene Hybrid-Ansatz (WebGL für Shapes, SVG für Connectors) ist technisch machbar, aber ein **architektonischer Albtraum** in Bezug auf Wartung und User Experience.

*   **Das Koordinaten-Problem:** WebGL nutzt (meist) Float-Koordinaten im GPU-Space, SVG nutzt DOM-Koordinaten. Bei jedem Zoom- oder Pan-Vorgang müssen beide Welten synchronisiert werden. Das führt unvermeidlich zu Latenz, "Drift" (Versatz zwischen Linie und Shape) und Flackern.
*   **Z-Index & Overlays:** Ein Shape muss unter-, über oder neben einem Connector liegen können. In einem Hybrid-System ist das Z-Indexing zwischen Canvas-Layer und DOM-Layer extrem schwer zu handhaben. Ein Connector, der *hinter* einem Shape verläuft, aber im SVG "on top" liegt, erfordert komplexe Masking-Strategien oder Splitting der SVG-Layer.
*   **Text-Rendering:** Dies ist der größte Schwachpunkt von WebGL. Text in PixiJS zu rendern erfordert entweder Bitmap-Fonts (pixelig bei Zoom) oder SDF (Signed Distance Fields) Fonts, die wiederum komplex sind und zusätzliche Build-Steps benötigen. Wenn Siqiy ein Werkzeug für Ingenieure ist, ist Text (Beschriftungen, Maße) first-class citizen. Canvas 2D rendert Text nativ und scharf.

**Empfehlung:**
Verwerfe den Hybrid-Ansatz. Wähle **einen** Renderer.

*   **Option A: Unified Canvas 2D (Empfohlen für Phase 1-3):**
    Nutze **Fabric.js** oder **Konva.js**. Beide bieten ein eingebautes Objektmodell (was für das ShapeSheet essenziell ist), hervorragendes Text-Rendering, Hit-Testing und Skalierung. Performante Canvas-Engines bewältigen heute problemlos 2.000–5.000 Objekte bei 60fps. Der Overhead von WebGL lohnt sich erst ab 10.000+ statischen Objekten (z.B. GIS), was für Diagramme selten der Fall ist.
*   **Option B: Pure WebGL (wenn extrem viele Objekte):**
    Wenn du zwingend WebGL willst, dann *nur* WebGL (z.B. mit **PixiJS** oder **Lumix**), aber du musst eine solide Text-Rendering-Lösung (SDF) bauen und auch die Connectors dort rendern. Keine Mischung.

## 2. Formel-Engine (HyperFormula)

**Analyse:**
HyperFormula ist eine großartige Bibliothek für Tabellenkalkulationen, aber es ist ein **semantic mismatch** für ein räumliches Diagramm-Tool.

*   **Grid-Kontext:** HyperFormula denkt in "Sheet1!A1". Siqiy denkt in "Shape42.Width". Das Mapping von Objekt-Properties auf ein virtuelles Grid ist künstlich, fehleranfällig und verschleiert den Code.
*   **Performance:** 2000 Shapes × 50 Cells = 100.000 Zellen. HyperFormula kann das, aber der Overhead einer kompletten Tabellenkalkulations-Engine (Parsing, Matrix-Logik) ist zu hoch für simple Abhängigkeiten wie `Width = Height * 1.5`.
*   **Typisierung:** HyperFormula ist stark auf Strings/Numbers fokussiert. Siqiy benötigt komplexe Objekte als Werte (z.B. eine Referenz auf ein anderes Shape-Objekt, ein Vektor, eine Farbe).

**Empfehlung:**
Baue einen **reaktiven Dependency-Graphen** auf Basis von **Signals** (z.B. Preact Signals, MobX, oder Solid.js CreateEffect).

*   Jede Shape-Cell ist ein `Signal`.
*   Eine Formel ist ein `Computed Signal`, das auf andere Signals zugreift.
*   Das ist leichtgewichtiger, typsicherer (TypeScript) und passt natürlicher zum Objektmodell als eine Grid-Engine.

## 3. Kollaboration & State (Yjs, Zustand)

**Analyse:**
*   **Yjs:** Die beste Wahl auf dem Markt. Reifes CRDT, hervorragende TypeScript-Unterstützung.
*   **Das Risiko (Reaktivität vs. CRDT):** Hier liegt das größte technische Risiko.
    *   *Szenario:* Nutzer A ändert `Shape1.Height`. Nutzer B hat eine Formel `Shape2.Width = Shape1.Height`.
    *   *Problem:* Yjs synchronisiert das *Ergebnis* der Formel oder die *Formel selbst*? Wenn Yjs die Formel synchronisiert, muss jeder Client die Formel neu auswerten. Wenn A die Formel ändert, während B den Wert ändert, entsteht ein Konflikt.
    *   *Lösung:* CRDTs sollten nur **Werte und Struktur** speichern (die "Source of Truth"). Die Formel-Auswertung (der "Derived State") muss rein lokal auf dem Client stattfinden. Das bedeutet: Die Formeln sind Teil des Dokuments (Yjs), die berechneten Werte nicht (oder nur als Cache).

**Empfehlung:**
*   Behalte **Yjs**.
*   Ersetze **Zustand** durch ein reaktives System, das direkt auf Yjs-Maps hören kann (z.B. `yjs-react` oder `observable-yjs`). Dupliziere den State nicht in Zustand – das führt zu Sync-Bugs. Der UI-State (z.B. "Ist das Menü offen?") kann in Zustand oder Context bleiben, aber der Dokument-State muss zentral aus Yjs kommen.

## 4. Connector-Routing (draw.io Code)

**Analyse:**
*   **Der Code-Bestand:** draw.io (mxGraph) ist mächtig, aber der Code ist ein Legacy-Monster (ursprünglich Java, dann GWT, dann transpiliert JS). Die Architektur ist weit von modernen Standards entfernt. Das "Extrahieren" des Routings ist oft schwieriger, als es neu zu schreiben, weil es tief in die DOM-Manipulation von mxGraph verflochten ist.
*   **Wartbarkeit:** Wenn du einen 5 Jahre alten Fork von draw.io-Code in dein modernes TypeScript-Projekt integrierst, wirst du jede Form von Code-Style und Typ-Sicherheit verlieren.

**Empfehlung:**
Verwende **ELK.js** (Eclipse Layout Kernel) oder implementiere einen sauberen **A* (A-Star) / JPS (Jump Point Search)** Algorithmus selbst.
*   Für orthogonales Routing gibt es etablierte Algorithmen (z.B. *Layered Graph Drawing* oder *Orthogonal Connector Routing*), die in ~500 Zeilen TypeScript modern und lesbar implementiert werden können.
*   Eine Spezialbibliothek wie **`hierarchy`** oder **`elkjs`** kann das Layout übernehmen. Für das interaktive Routing (während man zieht) ist ein schneller Grid-Search (A*) besser als der schwere draw.io-Stack.

## 5. Desktop & Framework (Tauri, React)

**Analyse:**
*   **Tauri:** Exzellente Wahl. Siqiy ist "Web-first". Tauri bietet einen kleinen Footprint und Rust-Sicherheit für Dateisystemzugriff.
*   **React:** React ist solide, aber für reaktive Canvas-Apps oft zu "heavy" im Render-Cycle, wenn man nicht aufpasst. Die UI (Toolbars, Properties Panel) ist perfekt in React. Aber der **Canvas-Loop** sollte von React entkoppelt sein (imperativ). Wenn du versuchst, jeden Frame via React-State zu steuern, wird es langsam.

**Empfehlung:**
*   **Tauri:** Beibehalten.
*   **React:** Beibehalten für die UI, aber strikte Trennung. Der Canvas-Editor sollte eine eigene Instanz/Klasse sein, die imperativ gesteuert wird (ähnlich wie `react-three-fiber`, aber für 2D).

## 6. Gesamtrisiko

**Das größte technische Risiko:**
Die **Interaktion zwischen Formel-Engine (Dependency Graph) und Kollaboration (CRDT)**.
Wenn Nutzer A die Basis-Formel löscht, auf die Nutzer B gerade gerade schreibt, und das System muss entscheiden, wie sich das auf abgeleitete Werte auswirkt, ohne in eine Endlosschleife oder Desynchronisation zu geraten.

**Schwerste rückgängig zu machende Entscheidung:**
Der **Hybrid-Rendering-Ansatz**. Wenn du erst einmal SVG-Overlays und WebGL-Koordinatensysteme verheiratet hast, ist es extrem schmerzhaft, das später zu trennen.

---

## Abschließende Empfehlung & Optimaler Stack

Hier ist der Stack, den ich für Siqiy empfehlen würde, um **Progressive Automation** und **Performance** in Einklang zu bringen:

### Der "Siqiy Optimal" Stack

| Komponente | Entscheidung | Begründung |
| :--- | :--- | :--- |
| **Core Language** | **TypeScript** | Unverzichtbar für die Komplexität von ShapeSheets. |
| **UI Framework** | **React + shadcn/ui** | Bewährtes Ökosystem für die Oberfläche. |
| **Rendering** | **Fabric.js (v6)** oder **Konva.js** | **Einheitlicher Canvas 2D**. Beide bieten ein integriertes Objektmodell (Gruppen, Events, Serialisierung), das perfekt zum ShapeSheet passt ("Objekt ist ein Container von Properties"). Text ist nativ scharf. Performance ist für <5k Objekte ausreichend. |
| **Formel-Engine** | **MobX 6** oder **Preact Signals** | Statt einer schweren Tabellenkalkulation nutzen wir ein feinkörniges Reaktivitätssystem. Jede Property einer Shape ist ein `Observable` / `Signal`. Das ist schneller, typsicherer und einfacher zu debuggen. |
| **Kollaboration** | **Yjs** | Der Industriestandard für Web-CRDTs. |
| **State Brücke** | **`yjs-mobx`** oder **Custom Adapters** | Verbindet das CRDT (Daten) mit dem Reaktivitätssystem (Formeln). Wenn sich das CRDT ändert, aktualisiert sich das Observable, die Formel feuert, der Canvas rendert. |
| **Connector Routing** | **Custom A* (Grid-Search) + ELK.js** | Ein leichtgewichtiger, selbst geschriebener A* für das interaktive Ziehen (schnell) und ELK.js für das automatische Layout/Arrange (mächtig). Vermeidung von Legacy-Code. |
| **Scripting** | **TypeScript (nativ)** + **Pyodide** | Wie geplant. |
| **Desktop** | **Tauri** | Wie geplant. |
| **Dateiformat** | **Yjs-Dokument (Binary) + JSON Export** | Speichere das native Yjs-Dokument (bietet History/Undo automatisch). Exportiere für Git/Human-Readability in sauberes JSON. |

### Zusammenfassung der Architektur-Änderungen

1.  **Kein Hybrid-Rendering:** Wir nutzen Canvas 2D. Das eliminiert die Koordinaten-Sync-Hölle und gibt uns perfektes Text-Rendering. Wir opfern die Fähigkeit, 100.000 Objekte zu rendern, gewinnen aber massively an Entwicklergeschwindigkeit und Stabilität im <5k Bereich.
2.  **Keine HyperFormula:** Wir nutzen ein reaktives Signal-System. Das modelliert "Shape A abhängig von Shape B" viel natürlicher als ein Tabellenblatt.
3.  **Kein draw.io Code:** Wir schreiben den Routing-Algorithmus sauber neu. Das ist eine Aufgabe von 1-2 Wochen, die jahrelange Wartungsschmerzen durch Legacy-Code spart.

Dieser Stack reduziert die "Magie" (schwer verständliche Black Boxes wie Hybrid-Rendering oder Grid-Mapping) und ersetzt sie durch "klar definierte Mechanismen" (Reaktivität, Canvas-Objekte), was genau der Philosophie von Siqiy entspricht: **Verstehen und Kontrollieren.**


