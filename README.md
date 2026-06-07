# TimeLog

Browser-only **PC-Arbeitstracking**. Eine einzige `index.html`, kein Backend, kein Login.
Sie pingt dich alle 15 Minuten — *"woran arbeitest du gerade?"* — und baut deinen Tag
als Blöcke in einer 3-Tage-Kalenderansicht auf. Export als Excel. Daten leben im
`localStorage` deines Browsers und überleben Reloads.

Gedacht als passives Pendant zu Pomodoro: nicht steuern, sondern tracken und reviewen.
Lücken sind gewollt — nicht am PC = kein Block.

## Nutzen

- Öffne `index.html` (lokal per Doppelklick oder via GitHub Pages).
- Alle 15 Minuten: Ping (Ton + Popup + optional OS-Benachrichtigung) → Stichwort eintragen
  oder **„Weiter wie eben"** oder leer lassen.
- Warst du weg? Beim Zurückkommen fragt ein **Catch-up** die letzten ~2 Stunden ab
  (oder „alle leer lassen").
- Blöcke anklicken zum Bearbeiten/Löschen. Mit ◀ ▶ durch die Tage blättern.
- **↓ Excel** exportiert `Datum | Wochentag | Start | Ende | Dauer | Tätigkeit` als `.xlsx`.

## Features

- 15-Minuten-Repeating-Ping mit Countdown-Ring, läuft real-time weiter.
- Catch-up für verpasste Pings (Cap 2 h), Slots einzeln oder gesammelt füllen.
- 3-Tage-Kalender im Google-Calendar-Stil, „jetzt"-Linie, aktueller Slot markiert.
- Deterministische Farben pro Tätigkeit (gleiches Stichwort = gleiche Farbe).
- Quick-Picks der letzten Tätigkeiten.
- `.xlsx`-Export (SheetJS) mit Datumsfilter.
- Persistenz via `localStorage`, übersteht Reloads.

## Tech

Vanilla HTML/CSS/JS in einer Datei. [SheetJS](https://sheetjs.com) via CDN für den
Excel-Export (braucht dafür Internet). Sonst keine Abhängigkeiten, kein Build.

## Deployment (GitHub Pages)

Repo → Settings → Pages → Source: `main` / root. Fertig — `index.html` ist die Seite.

## Lizenz

MIT — siehe [LICENSE](LICENSE).
