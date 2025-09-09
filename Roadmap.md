# Roadmap – Household Copilot (Android/Kotlin + GitHub + Codex)

## Vision
Eine native Android‑App, die tägliche/wöchentliche Aufgaben, Einkaufen, Rezepte/Mealplan und generelle Haushaltsorganisation steuert. 
Ohne eigenen Server: Daten leben im GitHub‑Repo. Die App ist read‑only gegenüber GitHub, Codex pflegt die Daten (Tasks, Rezepte, Pläne) per Commit/PR ein. 
Fällige Aufgaben werden per Overlay‑Pop‑ups (über allen Apps) „nagging until done“ erinnert und lassen sich in eine Sammel‑Bubble zusammenfassen.

## Nicht‑Ziele (v1)
- Kein Schreiben ins Repo direkt aus der App.
- Keine Cloud‑Datenbank, kein Realtime‑Push; Pull‑basiert via WorkManager.
- Kein komplexes Benutzer‑/Rollenmodell (Single‑Owner Repo).
- Kein iOS‑Client in v1.

---

## Meilensteine (inklusiv Akzeptanzkriterien)

### M1 – Projektgrundlagen & Skeleton
- ✅ Android‑Projekt (Kotlin, minSdk 26, target 35).
- ✅ Module: `core` (Room, Domain), `sync` (Retrofit/Moshi, GitHub API), `reminder` (AlarmManager), `overlay` (ForegroundService + Bubble), `ui` (Einstellungen, Task-Board).
- ✅ Notification Channels, Runtime‑Permissions (POST_NOTIFICATIONS, SYSTEM_ALERT_WINDOW).
- **Akzeptanz:** App startet, zeigt Demo‑Task‑Liste; Overlay‑Service lässt sich einschalten und zeigt Bubble.

### M2 – GitHub‑Sync (Pull‑only, ETag)
- ✅ GitHub Contents API Client (Bearer Token optional; Public Repo ohne Token).
- ✅ ETag‑basierter Download für definierte Dateien/Ordner (`tasks/*.yaml`, `recipes/*.json` …).
- ✅ YAML/JSON‑Parser → Mapping auf Room‑Entities; idempotentes Upsert mit `sourceHash`.
- ✅ Periodischer Sync via WorkManager; manueller „Sync Now“.
- **Akzeptanz:** Änderung in Repo → Commit → App zieht neuen Stand, ohne Duplikate; Offline weiterhin nutzbar.

### M3 – Reminder‑Engine & RRULE
- ✅ RRULE‑Parser (Subset: FREQ=DAILY/WEEKLY, BYDAY, BYHOUR, BYMINUTE, INTERVAL).
- ✅ Berechnung `nextFireAt`; Persistenz pro Task.
- ✅ AlarmManager `setExactAndAllowWhileIdle` (falls erlaubt) + Fallback; Quiet Hours.
- ✅ „Nagging until done“ (periodischer Re‑Ping bis erledigt/Snooze).
- **Akzeptanz:** Daily/Wöchentlich Tasks feuern zuverlässig; Snooze/Erledigt wirkt sofort.

### M4 – Overlay‑UX (Sammler‑Bubble & Karten)
- ✅ Bubble (draggable, snap to edges).
- ✅ Card‑Stack mit fälligen Tasks (Erledigt, Snooze 5/15/60, Details/Expand).
- ✅ Blink/Vibration konfigurierbar; Quiet Hours respektiert.
- ✅ Ein‑Klick‑Zusammenfassen: alle Pop‑ups werden im Sammler dargestellt.
- **Akzeptanz:** Bei Fälligkeit blinkt Bubble, Tap → Stack; Bedienung mit Handschuh‑Daumen möglich.

### M5 – Kochen & Einkauf (Reader/Overlays)
- ✅ Rezept‑Viewer (Zutaten abhaken, Schritt‑Timer je Step).
- ✅ Einkaufslisten‑Ansicht; aus Rezepten ableitbar (Pantry‑Abgleich optional).
- ✅ Optionaler „Koch‑Overlay‑Modus“ (Sub‑Pop‑ups mit Anweisungen/Timern).
- **Akzeptanz:** Nutzer kann Rezept öffnen, Schritte/Timer sehen; fehlende Zutaten zur Liste hinzufügen.

### M6 – Stabilität, Sicherheit, Release
- ✅ Token‑Speicher: Android Keystore (encrypted prefs).
- ✅ Crash‑/ANR‑Logging, strukturierte Logs (Timings für Sync & Scheduler).
- ✅ Backup/Export (lokale JSON‑Kopie des Bundles).
- ✅ Alpha‑Release‑Build (Internal Test Track).
- **Akzeptanz:** 24h Testlauf ohne ANR/Crash; Logs zeigen Sync‑ETag‑Trefferquote ≥ 80%.

---

## Technische Leitplanken
- **Architektur:** Clean-ish: Domain/UseCases, Repositories, DataSources; UI per Activities/Fragments (oder Compose später).
- **Persistenz:** Room; Migrations mit `autoMigrations` wo möglich.
- **Sync:** Retrofit/Moshi; ETag/304; Backoff‑Strategie bei 403/5xx.
- **Reminder:** AlarmManager + BroadcastReceiver; „exact alarms“ nur wenn freigeschaltet.
- **Overlay:** `TYPE_APPLICATION_OVERLAY`, `FLAG_NOT_FOCUSABLE`, eine Service‑Instanz, mehrere Karten.

---

## Qualität & Tests
- **Unit:** Parser (YAML→Entities), RRULE Next‑Occurrence (Edgecases), ETag Store.
- **Instrumented:** Overlay‑Service Lifecycle, DB‑Migrations, WorkManager Scheduling.
- **E2E (manuell):** Repo‑Commit → GitHub Action → App zieht Bundle → Alarm feuert → Overlay blinkt → Erledigt.

---

## Risiken & Gegenmaßnahmen
- **Doze/Alarme gedrosselt:** Foreground‑Service + `setAlarmClock()` für kritische Fälle; Nutzerhinweis „Exact Alarms erlauben“.
- **GitHub Rate Limits:** ETag nutzen, Poll‑Intervall konfigurierbar, Directory‑Listing cachen.
- **Overlay‑Permission verweigert:** Degradieren auf Heads‑Up Notifications mit Full‑Screen‑Intent (sparsam).
- **Schema‑Drift:** JSON‑Schema + CI‑Validation im Repo (GitHub Actions).

---

## Backlog (nach v1)
- Geofencing (Supermarkt öffnet Einkaufsliste).
- Voice Quick Add (Offline STT).
- ICS‑Export/Import; Kalender‑Integration.
- Mehr RRULE‑Features (BYMONTHDAY, EXDATE).
- Multi‑User‑Repo (später).
