# Changelog

All notable changes to this fork are documented here.
Versions up to and including v4.1 are inherited from
[HallyAus/homeassistant-bambu-blueprints](https://github.com/HallyAus/homeassistant-bambu-blueprints).

---

## Archived

This project moved to
[dabo53ck/bambu-printer-notifications-blueprint](https://github.com/dabo53ck/bambu-printer-notifications-blueprint).
No further changes are made here. The changelog there continues from
`0.0.1-beta`.

---

## [Unreleased]

### Added

- **`time_format` input** (Notification Settings, default `24h`). The
  notification message now carries the print end time on both the success and
  the fault path. The input selects between `24h` (`14:05:09`) and `12h` AM/PM
  (`02:05:09 PM`); seconds are always shown. The time is rendered with
  `now().strftime(...)` in the post-wait `variables:` block, so it lines up
  with the re-read `progress` / `status` values rather than trigger time.
  Existing automations pick the input up on re-import without any change.

---

## [v5-fork] — first release of this fork

Reworked around the finding that print progress is an unreliable trigger for
the snapshot on a Bambu P1S. **Contains breaking changes** — see
*Removed* below and the migration section in the README.

### Added

- **Stage-based primary trigger.** Fires when the current-stage sensor leaves
  `printing`, `inspecting_first_layer` or `auto_bed_leveling`. On a P1S this
  is roughly 40 seconds ahead of the progress and status sensors, and marks
  the start of the window in which the build plate lowers. Guarded with
  `not_to: [unavailable, unknown]` so an MQTT dropout mid-print does not
  register as the end of a print.
- `trace: stored_traces: 20`. The default of 5 was regularly exhausted by the
  triggers of a single print, evicting the traces needed for diagnosis.

### Changed

- **`mode: single` instead of `queued`** (with `max_exceeded: silent`).
  All triggers write to the same snapshot filename and use the same
  notification tag; under `queued` the later triggers overwrote a correctly
  timed snapshot about 400 ms after the notification had been sent, and
  replaced the notification on the device.
- **`wait_for_trigger` guard** now checks for print status `running` rather
  than for `finish`/`failed`. `finish` is frequently a short-lived
  intermediate state already superseded by `idle` at the time of the check,
  which caused the automation to sit out the full 10-minute timeout
  (upstream issue #7).
- **`progress`, `status` and `print_weight` are re-read after the wait**, so
  the notification reports the actual final values instead of those captured
  at trigger time. With the earlier stage trigger this is the difference
  between reporting `97 %` and `100 %`.
- **Progress trigger is now a hardcoded fallback** at `above: 99`, reached
  only when the stage trigger does not fire.
- **Notification body no longer carries YAML indentation.** Previously every
  line after the first was prefixed with two spaces on the device.
- **Critical time windows:** identical start and end time now means *never
  critical*, matching the description in the upstream v2 notes. The previous
  behaviour was the opposite (critical all day). Success window defaults
  changed from `00:00`–`00:00` to `07:00`–`21:00`, aligning them with the
  fault window.

### Removed

- **TTS announcements** — all inputs, variables and the action block. The
  `tts.speak` branch passed the service name where a TTS *entity* was
  required and therefore could not work; `continue_on_error: true` hid the
  failure. The quiet-hours check also ran eight `strptime` calls on every
  trigger regardless of whether TTS was enabled. Use the custom success and
  fault actions for announcements instead.
- **Cooldown condition and its input.** Demonstrably never blocked a run, and
  `mode: single` covers the case it was intended for.
- **Configurable progress threshold input.** A user-facing threshold no longer
  made sense: the stage trigger always fires first, and setting the threshold
  to 100 silently disabled the trigger entirely, since `numeric_state` with
  `above: 100` can never fire.
- **`startup` and `automation_reloaded` triggers**, along with the `stop`
  action that cancelled them. They performed no work and consumed trace slots.
- **Unused top-level variables** `status` and `print_weight`.
- **`paused`** from the in-progress stage guard — the Bambu stage sensor
  reports `paused_nozzle_clog`, `paused_user` and similar, never a bare
  `paused`.

---

## [v4.1] — upstream

- Fixed spurious notifications when the printer reconnects from an offline state
- Added `from` constraints on state triggers
- Added a guard condition rejecting triggers from offline/unavailable states

## [v4] — upstream

- TTS announcements with customisable messages
- TTS quiet hours
- Support for multiple TTS services

## [v3] — upstream

- Custom actions on success and fault
- Inputs organised into collapsible sections

## [v2] — upstream

- Optional snapshot light with brightness control
- Critical notifications made truly optional
- Snapshot delay restricted to positive values
- Time window handling reworked

## [v1] — upstream

- Initial release
