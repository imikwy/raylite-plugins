# raylite-plugins

Plugin-Index für [raylite](https://github.com/imikwy), einen Tastatur-Launcher
für Windows. raylite lädt `registry.json` von hier, höchstens einmal pro Tag
(oder beim manuellen Aktualisieren), und hält den letzten Stand lokal vor.

> Installiert wird in raylite im **Plugin Store** (Befehl „Plugin Store“ im
> Launcher, Tray-Menü oder Store-Knopf in der Befehlsleiste): Permissions
> bestätigen, raylite prüft die SHA-256-Prüfsumme und entpackt nach
> `%APPDATA%\raylite\plugins\<id>\`. `wetter`, `farbwaehler` und `musik` sind
> **Beispiel-Einträge** ohne Download.

## Format von `registry.json`

Ein JSON-Array. Jeder Eintrag:

| Feld | Pflicht | Bedeutung |
| --- | --- | --- |
| `id` | ja | Ordnername nach der Installation: `a–z`, `0–9`, `-`, höchstens 64 Zeichen, eindeutig |
| `name` | ja | Anzeigename |
| `version` | ja | `x.y.z` |
| `author` | ja | Autor bzw. Herausgeber |
| `description` | nein | kurze Beschreibung |
| `category` | nein | `development`, `productivity`, `media` oder `system` – fehlt sie oder ist sie unbekannt, zeigt raylite den Eintrag unter „Sonstiges“ |
| `repository_url` | ja | Quellcode (nur `https://`) |
| `download_url` | ja | ZIP mit `manifest.json` im Wurzelverzeichnis (nur `https://`) |
| `icon_url` | nein | Icon (nur `https://`) oder `null` |
| `permissions` | nein | wie im Manifest, zur Anzeige vor der Installation |
| `min_raylite_version` | ja | `x.y.z` – ältere raylite-Versionen zeigen den Eintrag als inkompatibel |
| `checksum` | ja | `sha256:<64 Hex-Zeichen>` der Datei hinter `download_url` |

Ungültige Einträge überspringt raylite einzeln. Ist keiner gültig oder die
Datei kein JSON-Array, behält raylite den zuletzt geladenen Stand.

## Plugin hinzufügen

1. Plugin-Ordner (mit `manifest.json`) als ZIP packen, Dateien im Wurzelverzeichnis:
   ```powershell
   Compress-Archive -Path .\mein-plugin\* -DestinationPath .\plugins\mein-plugin-1.0.0.zip
   ```
2. Prüfsumme berechnen:
   ```powershell
   (Get-FileHash .\plugins\mein-plugin-1.0.0.zip -Algorithm SHA256).Hash.ToLower()
   ```
3. Eintrag in `registry.json` ergänzen, `checksum` = `sha256:` + Ergebnis.

Aufbau eines Plugins: siehe `plugins/README.md` im raylite-Projekt.
