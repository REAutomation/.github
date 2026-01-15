# Development Rules - RE Automation GmbH

> Verbindliche Entwicklungsrichtlinien fuer alle Projekte

---

## Sprache

| Kontext | Sprache |
|---------|---------|
| Kommunikation, Dokumentation | Deutsch |
| Code, Variablen, Logs, Kommentare | Englisch |
| UI-Texte, Uebersetzungsdateien | Nach Anforderung |

---

## Datei-Management

### Benennung

| Regel | Beispiel |
|-------|----------|
| Keine Suffix-Dateien fuer Arbeitsdateien | ~~component-fixed.tsx~~ |
| Originalname beibehalten | `component.tsx` |
| Backup mit Timestamp | `component.backup-2024-11-16.tsx` |
| Obsolete Dateien | In `/old` Ordner verschieben |

### Ordnerstruktur

- Logische Hierarchie mit aussagekraeftigen Namen
- Keine Dateien im Root verstreuen
- Klare Trennung: `src/`, `docs/`, `old/`, `tools/`

---

## Code-Qualitaet

### Grundregeln

- **Keine Platzhalter, Mocks oder leere Stubs** - nur vollstaendige Implementation
- Ausnahme: Nur auf explizite Anweisung
- Keine Hungarian Notation
- OpenPLC Naming Conventions verwenden

### Datei-Header

Jede Quelldatei benoetigt einen Header:

```
/**
 * RE Automation GmbH
 * Copyright (c) 2026
 *
 * [Kurzbeschreibung der Datei]
 *
 * [Besondere Hinweise falls noetig]
 */
```

---

## CODESYS-spezifisch

Nach **jeder** CODESYS-Code-Aenderung:

1. Checker auf ALLEN CODESYS-Code-Dateien ausfuehren
2. GUID-Eindeutigkeit wird automatisch geprueft
3. Iterativ fixen bis keine Fehler mehr
4. GUIDs muessen innerhalb des CODESYS-Projekts eindeutig sein

---

## Dokumentation

### Struktur

```
projekt/
+-- docs/                    <- Hauptdokumentation (bidirektional)
|   +-- doc-01-overview.md
|   +-- doc-02-architecture.md
|   +-- doc-15-lessons-learned.md
|   +-- ...
|
+-- CLAUDE.md                <- NUR Session-Recovery
+-- README.md                <- Projekt-Uebersicht
```

### CLAUDE.md Management

**CLAUDE.md MUSS kompakt bleiben - NUR fuer Session-Recovery!**

**Was REIN gehoert:**
- Aktueller Status (wo stehen wir JETZT)
- Letzte durchgefuehrte Schritte (max. 3-5)
- Naechste geplante Schritte
- Lessons Learned - NUR kompakte Stichpunkte (1-2 Zeilen)
- Offene Issues/Blocker
- Referenzen auf relevante Dokumente

**Was NICHT rein gehoert:**
- Architektur-Dokumentation (-> `docs/`)
- API-Dokumentation (-> `docs/`)
- Vollstaendige Implementierungs-Details
- Alte, erledigte Status-Updates
- Duplikate von Informationen aus `docs/`

**Ziel:** < 500 Zeilen, ermoeglichst nahtloses Weiterarbeiten

### docs/ Ordner

- Bidirektional: Aenderungen muessen zurueckfliessen
- Version erhoehen bei Aenderungen
- Architektur, APIs, detaillierte Konzepte gehoeren hierhin

---

## Implementierungs-Workflow

### 0. VOR jeder Coding-Session

> **WICHTIG:** Checke die Lessons Learned in der CLAUDE.md!

### 1. Bei unbekannten Features/APIs

**NIEMALS raten wie eine API/Bibliothek funktioniert!**

```
1. ZUERST: Projekt-Dokumentation in /docs pruefen
2. DANN: Online-Dokumentation/Beispiele suchen und vollstaendig lesen
3. NUR WENN NICHTS GEFUNDEN: STOPP -> User fragen
4. NIEMALS: Implementation basierend auf Annahmen beginnen
```

**Bei mehreren Loesungswegen:**
- STOPP -> User praesentieren welche Optionen existieren
- User entscheidet

### 2. Testen & Validierung

**Nutze IMMER verfuegbare Tools zum selbststaendigen Testen!**

| Tool | Verwendung |
|------|------------|
| Chrome DevTools MCP | Frontend Screenshots/Inspektion |
| curl, Bash | Backend/API-Tests |
| npm, pip | Dependencies selbst installieren |
| Linter/Checker | Code selbst pruefen |

**NUR User fragen wenn:**
- Admin-Rechte benoetigt werden
- Externe Systeme/Hardware involviert sind (PLC, Maschinen)
- Echte User-Entscheidungen noetig sind

### 3. File-Writing bei Problemen

1. Backup erstellen
2. Alternative Methoden probieren: Python-Script, sed, awk, direkter Write
3. Notfall: Chunks schreiben
4. **NIEMALS** "bitte aendere manuell Zeile X" als Loesung

### 4. Prozess-Management

> **KRITISCH: NIEMALS alle Node.js Prozesse killen!**

```bash
# VERBOTEN - beendet Claude Code Session!
killall node
pkill -9 node

# ERLAUBT
kill <PID>           # Spezifischer Prozess
```

---

## Transparenz bei Problemen

### NIEMALS "fertig" sagen wenn:

- Schritte uebersprungen wurden
- Tests nicht durchgefuehrt werden konnten
- Fehler ignoriert wurden
- Annahmen getroffen wurden statt zu verifizieren
- Teile der Implementation fehlen

### IMMER explizit melden:

- Was NICHT funktioniert hat
- Was uebersprungen wurde und warum
- Welche Tests NICHT durchgefuehrt wurden
- Welche Annahmen getroffen wurden
- Was tatsaechlich funktioniert und getestet wurde

### Beispiel korrekte Statusmeldung

```
Implementation abgeschlossen. Tests:
[x] Backend-API getestet via curl
[x] Datenbank-Migration erfolgreich
[ ] Frontend-Screenshot konnte nicht erstellt werden (Chrome DevTools Fehler: XYZ)
    -> Bitte manuell pruefen: http://localhost:3000

Status: Bereit fuer Review, Frontend visuell noch nicht verifiziert.
```

---

## GitHub & Versionskontrolle

### Organisation

- **URL:** https://github.com/REAutomation
- **Konventionen:** [REPO_NAMING.md](REPO_NAMING.md)
- **Sichtbarkeit:** Standardmaessig `private`

### Repository-Namenskonventionen

**Globale Repositories:**

| Typ | Pattern | Beispiel |
|-----|---------|----------|
| Bibliotheken | `lib-[name]-[stack]` | `lib-motion-codesys` |
| Templates | `template-[name]-[stack]` | `template-machine-codesys` |
| Interne Tools | `internal-[name]` | `internal-codesys-api` |

**Kundenprojekte:**

| Teil | Beschreibung | Beispiel |
|------|--------------|----------|
| Customer | Kundenabkuerzung | `mahle` |
| Projectnr | Format YYNNN | `25001` |
| Part | Maschinenkomponente | `main` |
| Stack | Technologie-Suffix | `-codesys` |

Vollstaendig: `mahle-25001-main-codesys`

### Technology Stack Suffixes

| Suffix | Technologie |
|--------|-------------|
| `-codesys` | CODESYS SPS-Projekte |
| `-tia` | Siemens TIA Portal |
| `-hmi-web` | Web-Visualisierung |
| `-backend` | Backend/Server |
| `-docs` | Dokumentation |
| `-config` | Konfiguration |
| `-embedded` | Firmware/Mikrocontroller |

### GitHub CLI

```powershell
# Installation
winget install GitHub.cli

# Authentifizierung
gh auth login

# Repo erstellen
gh repo create REAutomation/[name] --private --source=. --remote=origin --push
```

### Commit-Konventionen

- Commits auf Englisch
- Praegnante Beschreibung im Imperativ
- Bei groesseren Features: mehrzeiliger Body mit Details

```
Add user authentication endpoint

- Implement JWT token generation
- Add password hashing with bcrypt
- Create login/logout routes
```

---

## Parallele Agenten (Claude Code)

### Grundregel

Nutze zum Coden extra Agenten mit Sonnet, da diese am besten coden koennen und das deinen Context schont.

### Bei grossen Aufgaben

**Voraussetzung - Kontrollplan erstellen:**

1. Aufgabe in unabhaengige Teilbereiche zerlegen
2. Abhaengigkeiten identifizieren
3. Reihenfolge festlegen (sequenziell vs. parallel)
4. Einen Kontroll-Agenten bestimmen

**Regeln fuer parallele Arbeit:**

- Agenten arbeiten an VERSCHIEDENEN Dateien/Modulen
- Keine ueberlappenden Verantwortlichkeiten
- Klare Schnittstellen zwischen Agenten definieren
- EIN Agent koordiniert:
  - Ueberprueft Fortschritt
  - Integriert Teilergebnisse
  - Fuehrt finale Tests durch
  - Aktualisiert CLAUDE.md

### Wann parallel arbeiten

- Grosse Refactorings ueber mehrere Module
- Mehrere unabhaengige Features gleichzeitig
- Test-Erstellung parallel zur Implementation
- Dokumentation parallel zum Code

---

## Erfolgskriterien

Fertigmeldung erst wenn:

- [ ] 100% Completion
- [ ] Alle Tests bestanden (selbst durchgefuehrt!)
- [ ] CLAUDE.md aktualisiert
- [ ] Keine manuellen Schritte uebrig
- [ ] Bei paralleler Arbeit: Integration erfolgreich
- [ ] Transparente Meldung ueber uebersprungene Schritte

---

*Dokument-Version: 1.0*
*Stand: 2026-01-15*
*Maintainer: RE Automation GmbH*
