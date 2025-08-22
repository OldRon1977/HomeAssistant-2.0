# Home Assistant Konfiguration

## Setup
1. Kopieren Sie `secrets-template.yaml` zu `secrets.yaml`
2. Füllen Sie die Werte in `secrets.yaml` aus
3. Starten Sie Home Assistant neu

## Datei-Struktur
```
/config/
├── configuration.yaml          # Hauptkonfiguration mit Kommentar-Blöcken
├── automations.yaml            # Alle Automationen in Blöcken
├── scripts.yaml               # Scripts
├── scenes.yaml                # Szenen
├── ui-lovelace.yaml           # Dashboard Konfiguration
├── secrets.yaml               # Sensible Daten (NICHT im Git!)
├── secrets-template.yaml      # Template für secrets.yaml
├── packages/                  # Modulare Konfigurationen
│   ├── beleuchtung.yaml
│   ├── heizung.yaml
│   └── sicherheit.yaml
└── custom_components/         # Eigene Komponenten (optional im Git)
    # Hinweis: HACS Komponenten meist besser über HACS verwalten
```

## Kommentar-Block System
Alle YAML-Dateien verwenden strukturierte Kommentar-Blöcke:

```yaml
# === BLOCK: EINDEUTIGER_NAME ===
# Zweck: Beschreibung der Funktionalität
# Status: ✅ Getestet / 🔄 Neu / ⚠️ Experimentell
# Erstellt: YYYY-MM-DD
# Abhängigkeiten: [Andere Blöcke/Entitäten]

[HIER STEHT DER EIGENTLICHE CODE]

# === END BLOCK: EINDEUTIGER_NAME ===
```

## Git Workflow

### Vor jeder Arbeitssession
```bash
# Backup des aktuellen Stands
git add .
git commit -m "Backup vor Session $(date +%Y-%m-%d)"
```

### Nach erfolgreichen Änderungen
```bash
# Geänderte Dateien hinzufügen
git add configuration.yaml automations.yaml
git status

# Commit mit aussagekräftiger Nachricht
git commit -m "Feature: [Beschreibung der Änderung]"

# Bei Remote Repository
git push
```

### Bei Problemen - Rollback
```bash
# Zum letzten funktionierenden Commit zurück
git log --oneline -5
git reset --hard [commit-hash]

# Falls nötig: Home Assistant neu starten
ha core restart
```

## Entwicklungsrichtlinien

### 1. Immer testen vor Commit
```bash
# Konfiguration prüfen
ha core check

# Bei Erfolg: Commit erstellen
# Bei Fehlern: Änderungen korrigieren
```

### 2. Aussagekräftige Commit-Nachrichten
```
✅ Gut: "Automation: Bewegungslicht Wohnzimmer hinzugefügt"
❌ Schlecht: "Update"

Kategorien:
- Feature: [Neue Funktionalität]
- Fix: [Fehlerbehebung]
- Config: [Konfigurationsänderung]
- Dashboard: [UI-Änderungen]
- Backup: [Sicherung vor Änderungen]
```

### 3. Block-System einhalten
- Jede Automation/Konfiguration in eigenen Block
- Eindeutige Block-Namen verwenden
- Status und Abhängigkeiten dokumentieren
- Bei Änderungen: Nur betroffene Blöcke anpassen

## Sicherheit & Backup

### Sensible Daten
- `secrets.yaml` wird **NICHT** ins Git übertragen
- Alle Passwörter/API-Keys in `secrets.yaml`
- `secrets-template.yaml` zeigt benötigte Werte ohne echte Daten

### Automatische Backups
```bash
# Tägliches Backup-Script (optional)
#!/bin/bash
cd /config
git add .
git commit -m "Auto-backup $(date)"
git push
```

## Projekt-Dokumentation System

Diese Repository nutzt ein strukturiertes Dokumentationssystem für Chat-basierte Entwicklung:

- **Vollständige Dateien:** Immer komplette YAML-Dateien, nie nur Snippets
- **Block-Austausch:** Änderungen erfolgen blockweise
- **Chat-Übergabe:** Projektstand wird zwischen Sessions dokumentiert
- **Template-System:** Standardisierte Dokumentation für Kontinuität

## Debugging & Troubleshooting

### Häufige Befehle
```bash
# Logs anzeigen
tail -f /config/home-assistant.log

# Konfiguration prüfen
ha core check

# Service neu starten
ha core restart

# Git Status prüfen
git status
git log --oneline -10
```

### Bei Config-Fehlern
1. **Sofort:** `git status` prüfen
2. **Logs:** Fehlermeldung in `/config/home-assistant.log` finden
3. **Rollback:** `git checkout [letzte_funktionierende_datei]`
4. **Fix:** Fehler korrigieren und erneut testen

### Bei Git-Problemen
```bash
# Ungewollte Dateien aus Git entfernen
git rm --cached [datei]

# .gitignore nachträglich anwenden
git rm -r --cached .
git add .
git commit -m "Apply updated .gitignore"

# Merge-Konflikte (bei Remote Repository)
git pull --rebase
# Konflikte manuell lösen, dann:
git add .
git rebase --continue
```

## Naming Conventions

### Entitäten
```yaml
# Sensoren: sensor.[raum]_[funktion]
sensor.wohnzimmer_temperatur
sensor.bad_luftfeuchtigkeit

# Automationen: automation.[zweck]_[auslöser]
automation.licht_bewegung_wohnzimmer
automation.heizung_nachtabsenkung

# Helper: input_boolean.[zweck]_[status]
input_boolean.urlaub_modus
input_boolean.gast_anwesend
```

### Dateien
```yaml
# Block-Namen: [KATEGORIE]_[SPEZIFIKATION]
AUTOMATION_BEWEGUNGSLICHT_WOHNZIMMER
SENSOR_TEMPERATUR_AUSSEN
DASHBOARD_VIEW_BELEUCHTUNG
```

## Wartung & Updates

### Regelmäßige Aufgaben
- **Wöchentlich:** Home Assistant Updates prüfen
- **Monatlich:** Custom Components Updates (HACS)
- **Bei Problemen:** Git-History für Rollback nutzen

### Update-Workflow
```bash
# Vor Updates: Backup
git add .
git commit -m "Backup vor HA Update auf [Version]"

# Nach Updates: Testen und committen
ha core check
git add .
git commit -m "HA Update auf [Version] - funktioniert"
```

## Letzte erfolgreiche Konfiguration

### Stabile Version taggen
```bash
# Nach erfolgreichem Test aller Funktionen:
git tag -a v1.0-stable -m "Funktioniert: alle Automationen getestet $(date)"

# Verfügbare Tags anzeigen:
git tag -l

# Zu stabiler Version zurück:
git checkout v1.0-stable
```

### Status-Referenz
- **Letzter stabiler Tag:** `git describe --tags --abbrev=0`
- **Aktuelle Position:** `git log --oneline --decorate -5`
- **Funktioniert:** [Manuell nach Tests aktualisieren]

---

**Wichtig:** Diese README wird bei strukturellen Änderungen des Projekts aktualisiert. Bei Fragen zum Block-System oder Git-Workflow siehe Projekt-Dokumentation.