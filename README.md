# Mahjong — Ruhig & Klar

Eine deutschsprachige Mahjong-Solitaire-Web-App mit **einzigartigen Symbolen** statt der klassischen durchnummerierten Stein-Reihen. Jedes der 72 Symbole erscheint genau zweimal im Spiel — das Paar bildet die Übereinstimmung. Keine Bambus 1–9, keine doppelten Drachen, keine Wind-Sortierung. Nur klare, eindeutige Bilder.

Im selben Stil wie die Sudoku- und Backgammon-Apps: eine einzelne `index.html`, ohne Build-Schritt, ohne Abhängigkeiten, offline-fähig als PWA.

## Features

- **4 Spielbretter:**
  - **Garten** — 36 Steine, 1 Ebene (sehr leicht, zum Eingewöhnen)
  - **Drache** — 32 Steine, 2 Ebenen
  - **Pyramide** — 60 Steine, 3 Ebenen
  - **Schildkröte** — 96 Steine, 4 Ebenen
- **Lösbar garantiert:** Generierung simuliert das Lösen rückwärts — jedes Spiel kann durchgespielt werden.
- **72 einzigartige Symbole:** Tiere, Pflanzen, Früchte, Wetter, Dinge — keine Symbol-Reihen mit Nummerierungen.
- **Komfort:** Rückgängig (Strg+Z oder „Zurück"), Tipp (zeigt ein passendes Paar), Mischen (sortiert die übrigen Steine neu, falls festgefahren), Größe (klein/mittel/groß).
- **Anpassbar:** Heller/Dunkler Modus, Stoppuhr und Paar-Zähler abschaltbar, Vibration auf dem Handy.
- **Speicher:** Spielstand wird automatisch gesichert; man kann zwischen den Spielbrettern wechseln und später weiterspielen.
- **PWA:** Auf dem Android-Startbildschirm installierbar; läuft offline.

## Bedienung

- **Stein wählen:** auf einen freien Stein tippen
- **Paar entfernen:** zweiten Stein mit gleichem Symbol antippen
- **Auswahl aufheben:** denselben Stein nochmal antippen
- **Tipp:** zeigt kurz ein passendes Paar
- **Mischen:** wenn nichts mehr passt, werden die übrigen Symbole neu verteilt
- **Zurück:** macht das letzte Paar wieder rückgängig
- **Tastatur am PC:** `H` Tipp · `S` Mischen · `Strg+Z` Zurück

Ein Stein ist nur dann wählbar, wenn:
1. **kein Stein darüber liegt** (oben frei) **und**
2. **mindestens eine Seite** (links oder rechts) **frei** ist

## Lokal ausprobieren

Einfach `index.html` per Doppelklick im Browser öffnen — fertig.

> Hinweis: Wenn man die Datei lokal als `file://` öffnet, funktionieren PWA und Offline-Modus nicht (das geht nur über `http(s)://`). Zum lokalen Testen mit Server:
>
> ```bash
> npx http-server Mahjong -p 8088
> ```
>
> Dann http://localhost:8088 im Browser öffnen.

## Auf GitHub Pages hochladen

1. Neues GitHub-Repository anlegen, z. B. `mahjong`.
2. Die Dateien aus diesem Ordner committen und pushen:
   - `index.html`
   - `manifest.webmanifest`
   - `sw.js`
   - `icon.svg`
   - `README.md` (optional)
3. Im Repository **Settings → Pages**:
   - Source: `Deploy from a branch`
   - Branch: `main` (oder `master`), Folder: `/ (root)`
   - Speichern.
4. Nach 1–2 Minuten ist die Seite unter `https://<dein-user>.github.io/mahjong/` erreichbar.

### Auf Android als App installieren

1. Die GitHub-Pages-URL im Chrome-Browser öffnen.
2. Im Chrome-Menü (drei Punkte) auf **„Zum Startbildschirm hinzufügen"** tippen.
3. Bestätigen — die App erscheint mit dem Mahjong-Icon und startet ohne Browser-Leiste.

## Technisches

- Eine HTML-Datei mit eingebettetem CSS und JavaScript — keine Build-Tools, keine Pakete
- Generierungs-Algorithmus: Aufbau in umgekehrter Lösungs-Reihenfolge — bei jedem Schritt werden zwei aktuell freie Positionen aus dem (virtuell) vollen Brett herausgenommen und mit demselben, neu vergebenen Symbol versehen. Damit existiert immer mindestens eine valide Lösungs­sequenz.
- Service Worker mit Network-First für `index.html` (Updates greifen sofort) und Cache-First für Assets
- Spielstand & Einstellungen in `localStorage`
- Funktioniert ohne Internet, sobald einmal geladen

## Updates ausrollen

Wenn du Änderungen an `index.html` machst:
1. Pushen → GitHub Pages aktualisiert sich automatisch
2. Beim nächsten Öffnen lädt der Service Worker die neue Version
3. Wenn etwas hängt: in `sw.js` die Zeile `const CACHE = 'mahjong-vN'` hochzählen — das erzwingt eine Cache-Erneuerung
