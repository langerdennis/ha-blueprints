# Home Assistant Blueprints

A collection of reusable blueprints for [Home Assistant](https://www.home-assistant.io/) (`2024.8.0+`).

---

## Available Blueprints

### 🤖 Vacuum: Away Prompt (Actionable Notification)

Sends at most **once per day** within a configurable time window an interactive push notification to two smartphones (Home Assistant Companion App) once both residents have been away for a set duration and no cleaning has taken place today.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Flangerdennis%2Fha-blueprints%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fvacuum_away_notification.yaml)

**Direct Import URL:**
```text
https://github.com/langerdennis/ha-blueprints/blob/main/blueprints/automation/vacuum_away_notification.yaml
```

#### Features
- **3 Action Buttons in the Notification:**
  - `🏠 Full Clean` – Starts a full cleaning run (`vacuum.start`)
  - `🛋️ Main Areas` – Cleans selected Home Assistant areas (`vacuum.clean_area`) or raw room segment IDs (`app_segment_clean`)
  - `⏭️ Skip Today` – Dismisses the prompt for the rest of the day
- **Synchronous Dismissal (`clear_notification`):** As soon as either resident taps a button or returns home, the notification is automatically cleared on both devices (an already running clean is not interrupted).
- **Morning Fallback:** If both residents leave before the active time window starts (e.g. before `08:00`), the prompt is deferred until the start of the window.
- **Zone- & Restart-Safe:** Uses event-driven triggers instead of blocking `wait_for_trigger` calls and supports multiple vacuum instances in parallel.
- **Optional Extras:** Water tank empty notice appended to the message and optional override/guest-mode switches (`input_boolean`).

#### Prerequisites in Home Assistant
1. **Home Assistant Companion App** installed on both smartphones.
2. **One Date Helper (`input_datetime`):**
   - Go to **Settings → Devices & services → Helpers → + Create Helper → Date and/or time**
   - Select **Date** only (without time) and save.
3. **For Room Cleaning (`Main Areas`) – choose one method:**
   - **Option A (recommended):** Open your vacuum entity settings, use **Map vacuum segments to areas**, and select the desired areas in the blueprint.
   - **Option B:** Run `roborock.get_maps` under **Developer Tools → Actions**, note the room segment IDs, and enter them comma-separated in the blueprint (e.g. `16, 17, 18`).

#### Updating
To pull future updates from this repository, go to **Settings → Automations & scenes → Blueprints → `⋮` → Re-import blueprint** in Home Assistant. Existing automations and selected entities remain intact.
