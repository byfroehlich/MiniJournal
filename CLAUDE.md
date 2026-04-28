# MiniJournal — CLAUDE.md

## Projekt
Trading Journal als Offline-HTML-App. Ersetzt `Journal Vorlage v018.xlsb`.

## Branch
`claude/xlsb-to-html-offline-k8zem`

## Ziel
Zwei Dateien im Ordner:
- `index.html` — die komplette App (alle CSS + JS inline)
- `journal.json` — die Datenbank (Trades + Einstellungen)

Ordner kopieren = neues Journal. HTML austauschen = Update ohne Datenverlust.

## XLSB-Struktur (bereits analysiert)

### Sheets → App-Tabs
| Sheet | App |
|---|---|
| Neuer Trade | Formular-Tab |
| Journal | Tabellen-Tab |
| Dashboard | Dashboard-Tab |
| Einstellungen | Einstellungen-Tab |
| Hilfsblatt | Berechnungen intern in JS |

### Felder "Neuer Trade"
**Allgemein:** Datum, Session, Asset, Richtung, TV Link, Auslöser
**Trade-Daten:** Outcome, Size, P/L ($), Risiko ($), CRV (geplant)
**Strategie:** Strategie, Einstiegselement
**Psychologie:** Bewertung (1–10), Gefühl, Pros, Cons, Reflexion, Lerneffekt

### Journal-Spalten
Nr, Outcome, Datum, Wochentag (auto), Session, Asset, Richtung, Size, P/L, Risiko, CRV, R (auto = P/L/Risiko), Kontostand (auto = kumulativ), Strategie, Einstiegselement, Bewertung, Gefühl, TV Link
*(Auslöser, Pros, Cons, Reflexion, Lerneffekt → nur in Detail-Ansicht)*

### Dashboard KPIs (Zeile 1)
Gesamte P/L, Kontostand, Winrate, Profitfaktor, Gesamt-R, Ø CRV

### Dashboard KPIs (Zeile 2)
Max. Gewinnserie, Max. Verlustserie, Max. Wintrade, Max. Kostentrade, Ø Wintrade, Ø Kostentrade

### Dashboard Charts
- Kontostand Verlauf (Linienchart, pure Canvas)
- P/L pro Trade (Balkenchart, pure Canvas, grün/rot)

### Dashboard Performance-Tabellen (Tabs)
Nach: Wochentag, Session, Asset, Richtung, Outcome, Strategie, Einstiegselement
Spalten je Tabelle: Kategorie, Trades, Summe P/L, Summe R, Ø CRV, Winrate

### Einstellungen (konfigurierbare Listen)
- Sessions: Asia, London, NY AM, NY PM
- Assets: ES, MES, NQ, MNQ, CL, MCL, GC, MGC, 6E, M6E
- Strategien: Broken Model, Brot und Butter, DR / IDR, Korrelationen, Königsweg, Opening Range Gap, Retirement Setup, Smart Money, Wochengap
- Einstiegselemente: Algoblock, Breaker Block, CISDO, Eingebundene Lücke, Liqui-Level, Lücke, Mitigation Block, VIB, Wick
- Richtungen: Long, Short
- Outcomes: Wintrade, Kostentrade, Breakeven, No Trade
- Gefühle: Sehr gut, Gut, Neutral, Schlecht, FOMO, Überzeugend, Unsicher, Diszipliniert
- Start-Kontostand: 50000

## journal.json Struktur
```json
{
  "version": "1.0",
  "settings": { ... },
  "trades": [
    {
      "id": 1714000000000,
      "datum": "2024-01-15",
      "session": "NY AM",
      "asset": "ES",
      "richtung": "Long",
      "tvLink": "",
      "ausloser": "",
      "outcome": "Wintrade",
      "size": 1,
      "pl": 250.00,
      "risiko": 100.00,
      "crv": 2.5,
      "strategie": "Brot und Butter",
      "einstieg": "Breaker Block",
      "bewertung": 8,
      "gefuehl": "Gut",
      "pros": "",
      "cons": "",
      "reflexion": "",
      "lerneffekt": ""
    }
  ]
}
```

## Berechnungen
- `R` = pl / risiko (auto)
- `Wochentag` = aus Datum (auto)
- `Kontostand` = startKontostand + kumulatives P/L
- `Winrate` = Winntrades / (Winntrades + Kostentrades) × 100
- `Profitfaktor` = Summe(Wins) / |Summe(Losses)|
- Max. Serien: längste aufeinanderfolgende Win/Loss-Kette

## Technologie
- Reines HTML + CSS + JS (keine externe Bibliothek)
- File System Access API (Chrome) zum Lesen/Schreiben der JSON-Datei
- Charts: pure Canvas (kein Chart.js)
- Browser: Chrome (file:// Protokoll)

## Features
- [x] CLAUDE.md erstellt
- [ ] index.html — Block 1: HTML-Gerüst + CSS
- [ ] index.html — Block 2: JS State + File System
- [ ] index.html — Block 3: Berechnungen + Charts
- [ ] index.html — Block 4: Views (Dashboard, Form, Journal, Einstellungen)
- [ ] journal.json — Standard-Datenbank
- [ ] Commit + Push

## Datenverwaltung in der App
- Export JSON (Download)
- Import JSON (Datei laden)
- Alle Trades löschen (Reset, mit Bestätigung)
- Neues Journal: Ordner kopieren im Explorer

## Hinweise
- Kein Server, kein Node, kein Python
- File System Access API: Chrome fragt beim Start einmal nach Dateiberechtigung
- HTML und JSON sind getrennt → HTML ersetzen = Update ohne Datenverlust
