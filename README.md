# Home Assistant Blueprints

Sammlung wiederverwendbarer Blueprints für [Home Assistant](https://www.home-assistant.io/) (ab Version `2024.8.0`).

---

## Verfügbare Blueprints

### 🤖 Saugroboter: Abwesenheits-Abfrage (Actionable Notification)

Sendet maximal **1× täglich** innerhalb eines einstellbaren Zeitfensters eine interaktive Push-Benachrichtigung an zwei Smartphones (Home Assistant Companion App), sobald beide Personen außer Haus sind und heute noch nicht gereinigt wurde.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Flangerdennis%2Fha-blueprints%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fvacuum_away_notification.yaml)

**Direkte Import-URL:**
```text
https://github.com/langerdennis/ha-blueprints/blob/main/blueprints/automation/vacuum_away_notification.yaml
```

#### Funktionen
- **3 Auswahl-Aktionen direkt in der Push-Nachricht:**
  - `🏠 Komplett` – Startet eine vollständige Reinigung (`vacuum.start`)
  - `🛋️ Hauptbereiche` – Reinigt ausgewählte Home-Assistant-Bereiche (`vacuum.clean_area`) oder Segment-IDs (`app_segment_clean`)
  - `⏭️ Heute nicht` – Schließt die Abfrage für den restlichen Tag
- **Synchrones Schließen (`clear_notification`):** Sobald auf einem Gerät ein Button angetippt wird oder eine Person nach Hause zurückkehrt, wird die Benachrichtigung automatisch auf beiden Smartphones entfernt (eine bereits laufende Reinigung wird nicht abgebrochen).
- **Morgen-Fallback:** Verlassen beide Personen bereits vor Beginn des Zeitfensters (z. B. vor `08:00` Uhr) das Haus, wird die Abfrage zum Start des Zeitfensters nachgeholt.
- **Zonen- & Neustart-sicher:** Arbeitet rein Event-basiert ohne blockierendes `wait_for_trigger` und unterstützt auch mehrere Saugroboter-Instanzen parallel.
- **Optionale Zusatzfunktionen:** Wassertank-Leer-Hinweis im Push-Text sowie Sperrschalter (z. B. für Gäste- oder Urlaubsmodus).

#### Voraussetzungen in Home Assistant
1. **Home Assistant Companion App** auf beiden Smartphones eingerichtet.
2. **Ein Datums-Helfer (`input_datetime`):**
   - *Einstellungen → Geräte & Dienste → Helfer → `+ Helfer erstellen` → Datum und/oder Uhrzeit*
   - Option **„Datum“** aktivieren (ohne Uhrzeit) und speichern.
3. **Für die Raumreinigung („Hauptbereiche“) – eine der beiden Varianten:**
   - **Variante A (empfohlen):** In den Einstellungen der `vacuum`-Entität die Saugroboter-Segmente den Home-Assistant-Bereichen zuordnen (*Segmente Bereichen zuordnen*) und im Blueprint die gewünschten Bereiche auswählen.
   - **Variante B:** Unter *Entwicklerwerkzeuge → Aktionen* die Aktion `roborock.get_maps` ausführen, die gewünschten Raum-Nummern ablesen und im Blueprint kommagetrennt eintragen (z. B. `16, 17, 18`).

#### Updates einspielen
Bei Änderungen in diesem Repository kann der Blueprint direkt in Home Assistant über *Einstellungen → Automatisierungen & Szenen → Blueprints → `⋮` → Blueprint erneut importieren* aktualisiert werden. Bestehende Automatisierungen und zugeordnete Entitäten bleiben dabei erhalten.
