# ⚡ Wallbox PV-Überschussladen

Intelligentes PV-Überschussladen für **go-e Charger** mit **Victron Cerbo GX**
in Home Assistant.

## ✨ Features

- **3 Lademodi:** PV-Überschuss, Schnellladen, Kein Laden
- **Batterie-Priorität:** Hausbatterie wird bevorzugt geladen
- **Automatische Phasenumschaltung:** 1-phasig ↔ 3-phasig
- **Geglättete Regelung:** Verhindert Flatterladen durch Durchschnittsbildung
- **Farbcodiertes Logging:** Alle Aktionen werden protokolliert
- **Log Viewer:** HTML-Viewer mit Dark Theme und Filterung
- **Blueprints:** Alles auch als wiederverwendbare Blueprints

## 📁 Dateistruktur

```
wallbox-pv-surplus/
├── README.md
├── packages/
│   └── wallbox_pv_surplus.yaml      ← Hauptpaket (alles-in-einer-Datei)
├── blueprints/
│   └── automation/
│       ├── wallbox_pv_control.yaml   ← PV-Überschuss Regelung
│       ├── wallbox_phase_up.yaml     ← Phasenumschaltung 1→3
│       ├── wallbox_phase_down.yaml   ← Phasenumschaltung 3→1
│       ├── wallbox_fast_charge.yaml  ← Schnellladen
│       └── wallbox_logger.yaml       ← Logger
├── www/
│   └── wallbox_log_viewer.html       ← Log Viewer (Dark Theme)
└── dashboard/
    └── wallbox_dashboard.yaml        ← Dashboard zum Kopieren
```

## 🚀 Installation

### Schritt 1: Package aktivieren

In `configuration.yaml` Packages aktivieren (falls noch nicht geschehen):

```yaml
homeassistant:
  packages: !include_dir_named packages
```

### Schritt 2: Dateien kopieren

```bash
# Package
cp packages/wallbox_pv_surplus.yaml  → /config/packages/

# Blueprints
cp blueprints/automation/*.yaml      → /config/blueprints/automation/wallbox/

# Log Viewer
cp www/wallbox_log_viewer.html       → /config/www/
```

### Schritt 3: Entity-IDs anpassen

Öffne `packages/wallbox_pv_surplus.yaml` und ersetze alle mit
`⚠️ ANPASSEN` markierten Stellen.

### Schritt 4: File-Notification einrichten

In `configuration.yaml`:

```yaml
notify:
  - platform: file
    name: wallbox_log
    filename: /config/www/wallbox_log.txt
    timestamp: false
```

### Schritt 5: Log-Datei erstellen

```bash
touch /config/www/wallbox_log.txt
```

### Schritt 6: Neustart

Home Assistant neu starten → Konfiguration prüfen → fertig! 🎉

## 🗺️ Entity-Mapping

Ersetze `goe_XXXXXX` mit deiner go-e Seriennummer.

| Platzhalter | Beschreibung | Beispiel |
|---|---|---|
| `sensor.victron_grid_power` | Victron Netz-Leistung (W) | `sensor.cerbo_grid_power` |
| `sensor.victron_battery_power` | Victron Batterie-Leistung (W) | `sensor.cerbo_battery_power` |
| `sensor.victron_battery_soc` | Victron Batterie SoC (%) | `sensor.cerbo_battery_soc` |
| `sensor.goe_XXXXXX_nrg_12` | go-e Wallbox Leistung (W) | `sensor.goe_ABC123_nrg_12` |
| `select.goe_XXXXXX_frc` | go-e Force-State | `select.goe_ABC123_frc` |
| `number.goe_XXXXXX_amp` | go-e Ampere | `number.goe_ABC123_amp` |
| `select.goe_XXXXXX_psm` | go-e Phasenmodus | `select.goe_ABC123_psm` |
| `binary_sensor.goe_XXXXXX_car_0` | go-e Auto verbunden | `binary_sensor.goe_ABC123_car_0` |
| `notify.wallbox_log` | File-Notification Entity | `notify.wallbox_log` |

## ⚙
