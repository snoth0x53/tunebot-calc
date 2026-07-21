# Tune-Bot Lug-Rechner

Ein kleiner, eigenständiger Web-Rechner zum Stimmen von Schlagzeugfellen nach der
Tune-Bot-Formel von Overtone Labs. Kein Build-Step, kein Backend – läuft komplett
im Browser.

🔗 **Live:** https://snoth0x53.github.io/tunebot-calc/

## Was das Tool macht

- Grundton (Note + Oktave) und Resonance-Modus pro Trommel einstellen
- Automatische Berechnung der Ziel-Lug-Frequenzen für Schlag- und Resonanzfell
- Durchmesser und Tiefe frei eingeben (Komma oder Punkt als Dezimaltrennzeichen)
- Tiefen-Heuristik: schätzt den Einfluss von Kesseltiefe auf die nötige Fellspannung
- **Kalibrierfunktion:** eigene gemessene Werte eintragen (Lug-Frequenz + reale
  Fundamentalfrequenz) und damit die Schätzung durch echte Messwerte ersetzen
- **Kreuz-Stimmreihenfolge:** Lug-Kreis zeigt die empfohlene Stimm-Reihenfolge
  (nicht die physische Position), automatisch passend zur eingestellten Lug-Anzahl –
  funktioniert für jede gerade Anzahl (6, 8, 10, 12 ...)
- **Snare-spezifische Formel:** eigener Rechenweg für Snares (Intervall-basiert:
  Quinte/Quarte/Terz/Unisono), getrennt von der Resonance-Modus-Logik für Toms/Bassdrum
- **Profi-Durchschnitt:** zusätzlicher Modus je Trommelart, basierend auf einer eigenen
  Auswertung von über 90 realen Artist-Tunings (siehe Abschnitt weiter unten)
- **Mikrofon-Messung:** ein "🎤 Messen"-Button pro Trommel misst den tatsächlichen
  Ist-Wert per YIN-Algorithmus (läuft bis manuell gestoppt, mehrfach anschlagen
  möglich) – Vergleich mit den drei Zielwerten erfolgt visuell (eingebautes oder
  externes Mikro, Geräteauswahl im Header)
- Ton-Vorschau: kurze synthetische Klangbeispiele für Schlagfell, Resonanzfell
  und den resultierenden Trommel-Grundton
- **Set-Name:** ganzes Kit benennen (z. B. "Rogers XP8"), um mehrere Drumsets
  auseinanderzuhalten
- **Drucken / Als PDF speichern:** aktuelle Einstellungen aller Trommeln als
  sauberes Dokument sichern oder ausdrucken (über den normalen Browser-Druckdialog)

## Grundton messen und kalibrieren

Der Grundton (Fundamentalfrequenz) ist die Tonhöhe der ganzen Trommel beim mittigen
Anschlag – nicht die Frequenz an einem einzelnen Lug. Ihn brauchst du für die
Kalibrierfunktion.

**Schritt 1 – Beide Felle unisono stimmen.**
Schlag- und Resonanzfell auf die gleiche Lug-Frequenz bringen. Die beiden Felle
stimmt man dabei getrennt: die jeweils andere Seite auf eine weiche Unterlage legen
(dämpft sie ab, damit nur das freie Fell schwingt), das freie Fell rundum gleichmäßig
stimmen, dann die Trommel umdrehen und das zweite Fell genauso stimmen.

**Schritt 2 – Grundton messen.**
Trommel auf einen Ständer stellen (nicht auf eine weiche Unterlage). Mittig aufs Fell
schlagen und den Gesamtklang messen – entweder mit dem Tunebot-Clip oder mit der
Mikrofon-Messung dieses Tools ("🎤 Messen"). Der abgelesene Wert ist deine reale
Fundamentalfrequenz.

**Schritt 3 – Kalibrieren.**
Diese gemessene Fundamentalfrequenz zusammen mit der eingestellten Lug-Frequenz in die
**Kalibrierfunktion** eintragen (siehe "+ Kalibrieren" bei jeder Trommel). Daraus wird
dein individueller Koeffizient berechnet, der die generische Formel für genau diese
Trommel und dieses Fell ersetzt – genauer als die Standardwerte.

## Formel-Grundlage

Basiert auf dem offiziellen [Tune-Bot Tuning Guide](https://tune-bot.com/tunebottuningguide.pdf)
von Overtone Labs (2012):

- **Maximum Resonance** (Unisono): Lug-Frequenz beider Felle = Grundton × 1,75
- **High Resonance:** höheres Fell × 1,85, tieferes Fell × 1,5
- **Medium Resonance:** × 2,0 / × 1,4
- **Low Resonance:** × 2,3 / × 1,2

Für **Snares** gilt ein eigener, im PDF separat beschriebener Ansatz (nicht die
Resonance-Modi oben): Schlagfell ≈ Grundton × 1,4, Resofell = Schlagfell ×
musikalisches Intervall (Standard: Quinte ×1,5). Bestätigt gegen reale Werte eines
Tune-Bot-Geräts.

Die Tiefen-Heuristik in diesem Tool ist **keine offizielle Formel**, sondern eine
eigene, transparent gekennzeichnete Näherung – siehe Hinweis im Footer der Seite.
Für maximale Genauigkeit empfiehlt sich die Kalibrierfunktion mit echten Messwerten.

## Profi-Durchschnitt: eigene Analyse realer Artist-Tunings

Zusätzlich zu den offiziellen PDF-Formeln enthält das Tool einen Modus **"Profi-
Durchschnitt"**, der auf einer eigenen Auswertung von über 90 auf
[tune-bot.com/artists](https://tune-bot.com/artists/) veröffentlichten Tunings
professioneller Drummer basiert. Kernergebnisse:

- **Snare (n=22):** Der PDF-Standard (Perfekte Quinte, Verhältnis 1,5) liegt über
  dem realen Durchschnitt (≈1,29) – die meisten Profis stimmen enger, näher an
  Terz/Quarte.
- **Tom (n=60):** Der bereits im Tool verwendete "High"-Modus passt schon gut zur
  Praxis (berechnetes Verhältnis 1,233 vs. beobachtet 1,20).
- **Bassdrum (n=14):** Das PDF-Beispiel setzt das Schlagfell exakt auf den
  Grundton (Verhältnis 1,0) – real liegt der Durchschnitt bei ≈1,62, spürbar
  straffer gestimmt.

Die vollständigen Rohdaten und Berechnungen liegen als CSV bei:
[`data/tune-bot-artist-tunings-analyse.csv`](data/tune-bot-artist-tunings-analyse.csv)
(alle Einzelwerte) und
[`data/tune-bot-analyse-zusammenfassung.csv`](data/tune-bot-analyse-zusammenfassung.csv)
(verdichtete Statistik). Offensichtliche Scraping-Duplikate auf der Artists-Seite
wurden vor der Mittelwertbildung markiert bzw. entfernt.

## Mikrofon-Test-Tool

`mic-test.html` ist ein eigenständiges Diagnose-Werkzeug, um ein Mikrofon-Setup zu
prüfen, bevor es im Rechner zum Einsatz kommt: Live-Wellenform, Frequenzspektrum,
Pegel-Meter und laufende Tonhöhenerkennung per YIN-Algorithmus, plus ein Log der
letzten 10 erkannten Anschläge zum Vergleich der Konsistenz zwischen Mikrofonen.

🔗 **Live:** https://snoth0x53.github.io/tunebot-calc/mic-test.html

**Wichtig:** Mikrofonzugriff funktioniert nur in einem echten Browser-Tab über
HTTPS (oder `localhost`) – nicht in eingebetteten Vorschau-Fenstern. Für lokale
Tests ohne Upload: `python3 -m http.server 8000` im Ordner mit der Datei, dann
`http://localhost:8000/mic-test.html` öffnen. Fürs iPhone im selben lokalen Test
zusätzlich ein HTTPS-Tunnel nötig (z. B. `cloudflared tunnel --url http://localhost:8000`).

## Genauigkeit der Mikrofon-Messung

Die Tonhöhenerkennung nutzt den **YIN-Algorithmus** (de Cheveigné & Kawahara, 2002) –
den Industriestandard für Pitch-Detection, robuster gegen Oktavfehler als einfache
Autokorrelation, mit eingebauter Zuverlässigkeitsschätzung (Confidence). Zusätzliche
Maßnahmen gegen Fehlmessungen:

- **Plausible Frequenzbereiche je Trommelart:** Bassdrum 30–160 Hz, Toms 50–350 Hz,
  Snare 50–450 Hz – filtert unrealistische Werte automatisch heraus
- **Median-Glättung** über die letzten 5 gültigen Messungen, um einzelne Ausreißer
  abzufedern
- Werte werden nur übernommen, wenn die YIN-Confidence einen Mindestwert überschreitet

Trotzdem bleibt die Mikrofon-Hardware der limitierende Faktor: Frequenzgang,
Abstand zum Fell und Raumgeräusche wirken sich stärker aus als jede Software-
Optimierung. Ein externes Mikrofon näher am Fell liefert zuverlässigere Werte als
das eingebaute Laptop-/Handy-Mikrofon aus größerer Distanz.

## Technik

Einzelne `index.html`-Datei: React + ReactDOM per CDN eingebunden, Tailwind CSS
per CDN, der eigentliche App-Code ist vorab zu reinem JavaScript kompiliert
(kein Live-Babel im Browser mehr nötig – robuster gegenüber Content-Blockern
und Erweiterungen). Kein npm, kein eigener Build-Schritt beim Deployen, direkt
als statische Seite über GitHub Pages hostbar.

## Nutzung lokal

`index.html` lässt sich direkt im Browser öffnen (auch per Doppelklick) – für alle
Funktionen außer der Mikrofon-Messung reicht das (Internetverbindung für die
CDN-Skripte wird benötigt). Für den "🎤 Messen"-Button braucht es wie beim
Mikrofon-Test-Tool einen sicheren Kontext (HTTPS oder `localhost`), siehe oben.

## Haftungsausschluss

Alle berechneten Frequenzen sind Richtwerte. Kesselmaterial, Felltyp, -dicke und
Bauart beeinflussen das tatsächliche Klangergebnis. Kein Ersatz für eigenes
Stimmen nach Gehör.
