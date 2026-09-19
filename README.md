# AI-Mark

Benchmark-Index für Sprachmodelle. Statische Seite, kein Build-Schritt, keine Abhängigkeiten.

## Grundregel

**Es stehen keine erfundenen Zahlen in diesem Projekt.** Modelle stehen im Array `BUILTIN` in `index.html`. Ein Modell erscheint erst in der
Rangliste, wenn der Betreiber seine acht Punkte einträgt. Kein Platzhalter, keine Schätzung, keine aus dem Gedächtnis
ergänzten Werte.

## Lokal ansehen

```bash
npm run dev        # http://localhost:3000
```

Oder `index.html` einfach im Browser öffnen.

## Deployen

Alle Varianten setzen einen Vercel-Account voraus.

**Variante A — Drag & Drop (schnellster Weg, ~30 Sekunden)**

1. Ordner mit diesen Dateien als ZIP packen (oder `ai-mark.zip` direkt verwenden)
2. https://vercel.com/new öffnen
3. ZIP bzw. Ordner in das Feld *Deploy a folder* ziehen
4. Deploy — fertig, die URL erscheint sofort

Keine CLI, kein GitHub, kein Login-Flow im Terminal. Nachteil: spätere Änderungen
müssen erneut hochgeladen werden.

**Variante B — CLI**

```bash
npx vercel login       # einmalig, öffnet den Browser
npx vercel             # Preview-Deployment
npx vercel --prod      # Produktion
```

**Variante C — Git-Import (empfohlen für laufende Updates)**

1. Repo zu GitHub pushen
2. Auf vercel.com → *Add New…* → *Project* → Repo auswählen
3. Framework Preset: **Other**, Build Command: leer, Output Directory: `.`
4. Deploy — ab dann löst jeder Push ein neues Deployment aus

## Modell eintragen

In `index.html`, im Array `BUILTIN`:

```js
const BUILTIN = [
  {
    id:       "opus5",            // eindeutig, klein geschrieben
    name:     "Claude Opus 5",
    lab:      "Anthropic",
    hue:      18,                 // 0-359, Farbe der Signatur-Grafik (optional)
    released: "2026-05",
    context:  "500K",
    quelle:   "https://…",        // Beleg für die Punkte
    note:     "Ein Satz zum Modell.",
    s: { text:0, plan:0, verst:0, code:0, research:0, math:0, science:0, agent:0 }
  }
];
```

Alle acht Werte in `s` sind Pflicht, jeweils 0–100. Ein unvollständiger Eintrag wird
nicht angezeigt (Warnung in der Browser-Konsole), statt mit falschem Index aufzutauchen.
Der AI-Mark-Index wird automatisch als ungewichteter Mittelwert der acht Werte berechnet.

Das Modellbild muss nicht angelegt werden — die Signatur-Grafik wird deterministisch
aus `id` + `name` erzeugt (vier Motive: Ring, Bitmuster, Prisma, Balken).

## Kategorie ändern

Array `CATS` in `index.html`. Ein Eintrag braucht `key`, `label` und `desc`. Kategorien
hinzuzufügen oder zu entfernen wirkt sich automatisch auf Tabelle, Netzdiagramm,
Kategorienraster und Indexberechnung aus — aber jedes Modell braucht danach einen Wert
für den neuen `key`.

## Aufbau

```
index.html    komplette Seite: Markup, Styles, Logik, Daten
vercel.json   Static-Hosting-Konfiguration und Header
package.json  nur Convenience-Skripte, keine Dependencies
```
