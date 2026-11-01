# Himmel Live – Android-App

Nimmt dein aktuelles Hintergrundbild, du markierst den Himmel mit Punkten und Linien,
und die App ersetzt diesen Bereich laufend durch einen Himmel passend zu **Uhrzeit, Sonnenstand
und Live-Wetter an deinem Standort**.

## Was die App macht
- **Hintergrund laden** – liest dein aktuelles Hintergrundbild aus (oder **Bild wählen** aus der Galerie).
  Das Original wird in der App gespeichert, damit es nie "doppelt" verändert wird.
- **Himmel markieren** – Punkte tippen → sie werden mit Linien verbunden (Polygon).
  - Punkt ziehen = verschieben
  - Auf eine Linie tippen = neuen Punkt dazwischen einfügen
  - Punkte nahe am Bildrand rasten am Rand ein
  - Tipp: Horizont von **links nach rechts** abtippen, dann **„Oberer Rand“** → die Auswahl wird über die oberen Ecken geschlossen.
- **Vorschau** mit Zeit-Regler (jetzt bis +24 h) zum Ausprobieren von Tag, Abendrot, Nacht.
- **Anwenden** setzt das Bild als Hintergrund (optional auch Sperrbildschirm).
- **Automatisch** aktualisiert alle 15 Minuten (Android-Minimum), aber nur wenn sich sichtbar etwas geändert hat.

### Was im Himmel passiert
- Farbverlauf nach echtem Sonnenstand (Nacht → Dämmerung → Abendrot → Tag)
- Sonne mit Schein bei tiefem Stand, Mond mit richtiger Mondphase, Sterne bei klarer Nacht
- Wolken je nach Bewölkung (%), ziehen mit dem Wind langsam weiter
- Regen-/Nieselstriche, Schneeflocken, Nebelschleier, dunkler Himmel bei Gewitter
- Der Rest des Bildes (Landschaft/Häuser) wird nachts dunkler und bläulich, bei Sonnenuntergang warm

Wetter kommt von [Open-Meteo](https://open-meteo.com) – kostenlos, ohne API-Schlüssel.

## APK über GitHub bauen (ohne Android Studio)
1. Auf GitHub ein neues Repository anlegen (z. B. `SkyWallpaper`).
2. Den Inhalt dieses Ordners hochladen (Web: „Add file → Upload files“, den ganzen Inhalt reinziehen –
   auch den Ordner `.github`!).
3. Reiter **Actions** öffnen → der Workflow **„APK bauen“** startet automatisch (ca. 3–5 min).
4. Auf den fertigen Lauf klicken → unten bei **Artifacts** `SkyWallpaper-apk` herunterladen (ZIP mit der APK).
5. APK aufs Handy, öffnen, „Installation aus unbekannten Quellen“ erlauben.

> Hinweis: Beim Hochladen über die Web-Oberfläche werden Ordner mit Punkt (`.github`) manchmal nicht mitgenommen.
> Dann `.github/workflows/build.yml` per „Add file → Create new file“ anlegen und den Inhalt reinkopieren.

Oder mit Git:
```bash
git init && git add . && git commit -m "Himmel Live"
git branch -M main
git remote add origin https://github.com/DEINNAME/SkyWallpaper.git
git push -u origin main
```

## Mit Android Studio
Ordner öffnen → warten bis Gradle synchronisiert → ▶ Run.

## Berechtigungen
- **Standort** – für Wetter und Sonnenstand (ohne Standort wird Wien verwendet)
- **Hintergrund setzen**
- **Zugriff auf alle Dateien** (nur Android 13+) – Android erlaubt das Auslesen des aktuellen
  Hintergrundbilds nur damit. Wenn du das nicht willst: einfach **„Bild wählen“** benutzen.

## Technik
Kotlin, minSdk 26 (Android 8), keine externen Bibliotheken außer WorkManager.
| Datei | Aufgabe |
|---|---|
| `SkyEditorView.kt` | Bild anzeigen, Punkte/Linien bearbeiten |
| `SkyRenderer.kt` | Himmel, Wolken, Sterne, Mond, Regen malen |
| `SunCalc.kt` | Sonnenhöhe aus Ort und Zeit |
| `Weather.kt` | Open-Meteo abfragen |
| `Updater.kt` | Update-Ablauf + Hintergrund-Job alle 15 min |
| `Store.kt` | Original, Polygon, Einstellungen speichern |
