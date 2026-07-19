# Tune-Bot Lug-Rechner

Ein kleiner, eigenständiger Web-Rechner zum Stimmen von Schlagzeugfellen nach der
Tune-Bot-Formel von Overtone Labs. Kein Build-Step, kein Backend – läuft komplett
im Browser.

🔗 **Live:** https://snoth0x53.github.io/tunebot-calc/

## Was das Tool macht

- Grundton (Note + Oktave) und Resonance-Modus pro Trommel einstellen
- Automatische Berechnung der Ziel-Lug-Frequenzen für Schlag- und Resonanzfell
- Lug-Kreis-Visualisierung passend zur Lug-Anzahl der Trommel
- Tiefen-Heuristik: schätzt den Einfluss von Kesseltiefe auf die nötige Fellspannung
- **Kalibrierfunktion:** eigene gemessene Werte eintragen (Lug-Frequenz + reale
  Fundamentalfrequenz) und damit die Schätzung durch echte Messwerte ersetzen
- Ton-Vorschau: kurze synthetische Klangbeispiele für Schlagfell, Resonanzfell
  und den resultierenden Trommel-Grundton

## So ermittelst du den Grundton (Fundamentalfrequenz)

Der Grundton ist die Tonhöhe der ganzen Trommel beim mittigen Anschlag – nicht die
Frequenz an einem einzelnen Lug.

1. Trommel auf einen Ständer stellen (nicht auf den Schoß/eine weiche Unterlage legen)
2. Beide Felle grob gleichmäßig stimmen (unisono, ein beliebiger Ausgangswert reicht)
3. Mit dem Tunebot-Clip an einem Lug ansetzen, mittig auf das Fell schlagen
4. Angezeigten Wert ablesen – das ist deine reale Fundamentalfrequenz

Diesen Wert kannst du zusammen mit der aktuell eingestellten Lug-Frequenz in die
**Kalibrierfunktion** des Rechners eintragen (siehe "▶ Kalibrieren" bei jeder Trommel).
Daraus wird dein individueller Koeffizient berechnet, der die generische Formel für
genau diese Trommel/dieses Fell ersetzt – genauer als die Standardwerte.

## Formel-Grundlage

Basiert auf dem offiziellen [Tune-Bot Tuning Guide](https://tune-bot.com/tunebottuningguide.pdf)
von Overtone Labs (2012):

- **Maximum Resonance** (Unisono): Lug-Frequenz beider Felle = Grundton × 1,75
- **High Resonance:** höheres Fell × 1,85, tieferes Fell × 1,5
- **Medium Resonance:** × 2,0 / × 1,4
- **Low Resonance:** × 2,3 / × 1,2

Die Tiefen-Heuristik in diesem Tool ist **keine offizielle Formel**, sondern eine
eigene, transparent gekennzeichnete Näherung – siehe Hinweis im Footer der Seite.
Für maximale Genauigkeit empfiehlt sich die Kalibrierfunktion mit echten Messwerten.

## Technik

Einzelne `index.html`-Datei, React + Babel Standalone + Tailwind CSS jeweils per
CDN eingebunden. Kein npm, kein Build, direkt als statische Seite über GitHub
Pages hostbar.

## Nutzung lokal

Einfach `index.html` im Browser öffnen – funktioniert ohne lokalen Server
(Internetverbindung für die CDN-Skripte wird benötigt).

## Haftungsausschluss

Alle berechneten Frequenzen sind Richtwerte. Kesselmaterial, Felltyp, -dicke und
Bauart beeinflussen das tatsächliche Klangergebnis. Kein Ersatz für eigenes
Stimmen nach Gehör.
