# Fellton

Ein kleiner, eigenständiger Web-Rechner zum Stimmen von Schlagzeugfellen. Kein
Build-Step, kein Backend – läuft komplett im Browser.

🔗 **Live:** https://snoth0x53.github.io/tunebot-calc/

## Was das Tool macht

- Grundton (Note + Oktave) und Resonance-Modus pro Trommel einstellen
- Automatische Berechnung der Ziel-Lug-Frequenzen für Schlag- und Resonanzfell
- Durchmesser und Tiefe frei eingeben (Komma oder Punkt als Dezimaltrennzeichen)
- Tiefen-Heuristik: schätzt den Einfluss der Kesseltiefe auf die nötige Fellspannung
- **Kalibrierfunktion:** eigene gemessene Werte eintragen (Lug-Frequenz + reale
  Fundamentalfrequenz) und damit die Schätzung durch echte Messwerte ersetzen
- **Kreuz-Stimmreihenfolge:** Der Lug-Kreis zeigt die empfohlene Stimm-Reihenfolge –
  immer zum gegenüberliegenden Lug, dann eine Schraube weiter und wieder gegenüber,
  sodass sich die Spannung gleichmäßig verteilt
- **Snare-spezifische Formel:** eigener Rechenweg für Snares (Intervall-basiert:
  Quinte/Quarte/Terz/Unisono), getrennt von der Resonance-Modus-Logik für Toms/Bassdrum
- **Profi-Durchschnitt:** zusätzlicher Modus je Trommelart, basierend auf einer eigenen
  Auswertung von über 90 realen Artist-Tunings (siehe Abschnitt weiter unten)
- **Mikrofon-Messung:** ein "🎤 Messen"-Button pro Trommel misst den tatsächlichen
  Ist-Wert per FFT-Spektralanalyse, mit zuschaltbarem Filter für den kritischen
  Hochtonbereich
- Ton-Vorschau: kurze synthetische Klangbeispiele für Schlagfell, Resonanzfell
  und den resultierenden Trommel-Grundton
- **Set-Name:** ganzes Kit benennen (z. B. "Rogers XP8"), um mehrere Drumsets
  auseinanderzuhalten
- **Drucken / Als PDF speichern:** aktuelle Einstellungen aller Trommeln sichern

## Lug-für-Lug stimmen mit der Mikrofon-Messung

Die Mikrofon-Messung erkennt die Frequenz jedes Anschlags. Wo du anschlägst,
bestimmt, was gemessen wird:

- **Nah am Lug anschlagen** (2–3 cm vom Rand) → die Lug-Frequenz dieser Schraube.
  So bringst du alle Lugs nacheinander auf denselben Wert.
- **Mittig anschlagen** → der Grundton der ganzen Trommel.

Für ein sauberes Ergebnis pro Lug mehrfach anschlagen – das Tool sucht über mehrere
Anschläge den stabilsten wiederkehrenden Ton und blendet einzelne Ausreißer
(Raumreflexionen, kurze Obertöne) aus. Nach einer kurzen Pause startet die Erkennung
frisch, sodass du beim Wechsel zum nächsten Lug einfach weitermachen kannst. Die Snare
am besten mit ausgehängtem Teppich messen, damit das Rasseln nicht ins Signal kommt.

### Filter für den Hochtonbereich

Nach oben hin wird jede Messung ungenauer – der Übergang beginnt etwa ab 250–300 Hz
und wird ab 350 Hz deutlich. Zwei Gründe: Hoch gestimmte Felle klingen kürzer aus und
füllen das Analysefenster nicht mehr aus, und die Fell-Partialtöne rücken enger
zusammen (die dritte und vierte Mode liegen nur rund 7 % auseinander). Dasselbe
Problem betrifft auch Hardware-Stimmgeräte.

Dafür gibt es den Filter: Erst eine korrekte Messung machen, dann **Filter** drücken.
Dieser Wert wird zur Referenz, und ab dann werden nur noch Frequenzen im Bereich
±20 % berücksichtigt – Oktavfehler und tiefe Störpeaks fallen heraus. Bei größeren
Stimmänderungen einfach neu setzen; für jede Trommel gilt der Filter getrennt. Beim
Fellwechsel, wenn du von ganz unten hocharbeitest, lässt du ihn am besten aus.

Die Gruppen-Liste unter dem Messwert zeigt weiterhin alle erkannten Rohwerte, auch
die vom Filter verworfenen – so siehst du, wenn der Filter etwas Echtes ausblendet.

## Grundton messen und kalibrieren

Der Grundton (Fundamentalfrequenz) ist die Tonhöhe der ganzen Trommel beim mittigen
Anschlag – nicht die Frequenz an einem einzelnen Lug. Ihn brauchst du für die
Kalibrierfunktion.

**Schritt 1 – Beide Felle unisono stimmen.**
Schlag- und Resonanzfell auf die gleiche Lug-Frequenz bringen. Die beiden Felle
stimmt man dabei getrennt: die jeweils andere Seite auf eine weiche Unterlage legen
(dämpft sie ab, damit nur das freie Fell schwingt), das freie Fell rundum gleichmäßig
stimmen, dann die Trommel umdrehen und das zweite Fell genauso.

**Schritt 2 – Grundton messen.**
Trommel auf einen Ständer stellen (nicht auf eine weiche Unterlage). Mittig aufs Fell
schlagen und den Gesamtklang messen. Der abgelesene Wert ist deine reale
Fundamentalfrequenz.

**Schritt 3 – Kalibrieren.**
Diese gemessene Fundamentalfrequenz zusammen mit der eingestellten Lug-Frequenz in die
**Kalibrierfunktion** eintragen (siehe "+ Kalibrieren" bei jeder Trommel). Daraus wird
dein individueller Koeffizient berechnet, der die generische Formel für genau diese
Trommel und dieses Fell ersetzt – genauer als die Standardwerte.

Warum Unisono? Der Koeffizient ist das Verhältnis `a = Lug ÷ Fundamental`. Sind beide
Felle unterschiedlich gestimmt, gibt es zwei verschiedene Lug-Frequenzen und das
Verhältnis wäre nicht eindeutig. Im Unisono-Zustand gibt es genau eine.

## Formel-Grundlage

Die Multiplikatoren stammen aus dem
[Overtone Labs Tune-Bot Tuning Guide](https://tune-bot.com/tunebottuningguide.pdf)
(2012):

- **Maximum Resonance** (Unisono): Lug-Frequenz beider Felle = Grundton × 1,75
- **High Resonance:** höheres Fell × 1,85, tieferes Fell × 1,5
- **Medium Resonance:** × 2,0 / × 1,4
- **Low Resonance:** × 2,3 / × 1,2

Für **Snares** gilt ein eigener, dort separat beschriebener Ansatz (nicht die
Resonance-Modi oben): Schlagfell ≈ Grundton × 1,4, Resofell = Schlagfell ×
musikalisches Intervall (Standard: Quinte ×1,5).

Die Tiefen-Heuristik in diesem Tool ist **keine offizielle Formel**, sondern eine
eigene, transparent gekennzeichnete Näherung – siehe Hinweis im Footer der Seite.
Für maximale Genauigkeit empfiehlt sich die Kalibrierfunktion mit echten Messwerten.

## Profi-Durchschnitt: eigene Analyse realer Artist-Tunings

Zusätzlich enthält das Tool einen Modus **"Profi-Durchschnitt"**, der auf einer eigenen
Auswertung von über 90 auf [tune-bot.com/artists](https://tune-bot.com/artists/)
veröffentlichten Tunings professioneller Drummer basiert. Kernergebnisse:

- **Snare (n=22):** Der Standard (Perfekte Quinte, Verhältnis 1,5) liegt über
  dem realen Durchschnitt (≈1,29) – die meisten Profis stimmen enger, näher an
  Terz/Quarte.
- **Tom (n=60):** Der "High"-Modus passt schon gut zur Praxis (berechnetes
  Verhältnis 1,233 vs. beobachtet 1,20).
- **Bassdrum (n=14):** Das Guide-Beispiel setzt das Schlagfell exakt auf den
  Grundton (Verhältnis 1,0) – real liegt der Durchschnitt bei ≈1,62, spürbar
  straffer gestimmt.

Die vollständigen Rohdaten und Berechnungen liegen als CSV bei:
[`data/tune-bot-artist-tunings-analyse.csv`](data/tune-bot-artist-tunings-analyse.csv)
(alle Einzelwerte) und
[`data/tune-bot-analyse-zusammenfassung.csv`](data/tune-bot-analyse-zusammenfassung.csv)
(verdichtete Statistik).

## Mikrofon-Test-Tool

`mic-test.html` ist ein eigenständiges Diagnose-Werkzeug, um ein Mikrofon-Setup zu
prüfen, bevor es im Rechner zum Einsatz kommt: Live-Wellenform, Frequenzspektrum mit
markierten Peaks und beschrifteter Frequenzachse, Pegel-Meter, Peak-Liste und ein
Log der letzten Anschläge zum Vergleich der Konsistenz zwischen Mikrofonen. Ein
"Einfrieren"-Button hält die Anzeige zum bequemen Ablesen fest.

🔗 **Live:** https://snoth0x53.github.io/tunebot-calc/mic-test.html

**Wichtig:** Mikrofonzugriff funktioniert nur in einem echten Browser-Tab über
HTTPS (oder `localhost`) – nicht in eingebetteten Vorschau-Fenstern. Für lokale
Tests ohne Upload: `python3 -m http.server 8000` im Ordner mit der Datei, dann
`http://localhost:8000/mic-test.html` öffnen. Fürs iPhone im selben lokalen Test
zusätzlich ein HTTPS-Tunnel nötig (z. B. `cloudflared tunnel --url http://localhost:8000`).

## Genauigkeit der Mikrofon-Messung

Nach jedem Anschlag wird das Frequenzspektrum per FFT analysiert (Fenstergröße 32768):
Alle Peaks über einer Schwelle (18 dB unter dem stärksten) werden ermittelt und nach
spektraler Nähe gruppiert. Der Gruppierungsabstand skaliert mit der Frequenz (6 %,
mindestens 8 Hz), weil die Partialtöne nach oben hin enger zusammenrücken. Über
mehrere Anschläge hinweg wählt das Tool den Ton, der am konstantesten wiederkehrt
(Toleranz ca. 4 %). Das filtert springende Störpeaks heraus. Parabel-Interpolation
verfeinert die Auflösung auf unter 1 Hz.

Zusätzliche Maßnahmen:

- **Plausible Frequenzbereiche je Trommelart:** Bassdrum 30–160 Hz, Toms 50–350 Hz,
  Snare 50–450 Hz
- **Neustart nach Pause:** Bleibt eine kurze Zeit ein Anschlag aus, beginnt die
  Erkennung frisch – praktisch beim Wechsel von Lug zu Lug
- **Filter** (siehe oben) für den kritischen Hochtonbereich

Ein Vergleich gegen ein Hardware-Stimmgerät ergab im Normalbereich sehr ähnliche
Werte; oberhalb von etwa 300 Hz werden beide ungenauer.

Die Mikrofon-Hardware bleibt der limitierende Faktor: Frequenzgang, Abstand zum Fell
und Raumgeräusche wirken sich stärker aus als jede Software-Optimierung. Ein externes
Mikrofon näher am Fell liefert zuverlässigere Werte als das eingebaute Laptop-/Handy-
Mikrofon aus größerer Distanz.

## Technik

Einzelne `index.html`-Datei: React + ReactDOM per CDN eingebunden, Tailwind CSS
per CDN, der eigentliche App-Code ist vorab zu reinem JavaScript kompiliert
(kein Live-Babel im Browser). Kein npm, kein eigener Build-Schritt beim Deployen,
direkt als statische Seite über GitHub Pages hostbar.

## Nutzung lokal

`index.html` lässt sich direkt im Browser öffnen (auch per Doppelklick) – für alle
Funktionen außer der Mikrofon-Messung reicht das (Internetverbindung für die
CDN-Skripte wird benötigt). Für den "🎤 Messen"-Button braucht es wie beim
Mikrofon-Test-Tool einen sicheren Kontext (HTTPS oder `localhost`), siehe oben.

## Haftungsausschluss

Alle berechneten Frequenzen sind Richtwerte. Kesselmaterial, Felltyp, -dicke und
Bauart beeinflussen das tatsächliche Klangergebnis. Kein Ersatz für eigenes
Stimmen nach Gehör.
