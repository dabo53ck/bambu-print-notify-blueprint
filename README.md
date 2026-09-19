# Moved: Bambu Printer Notifications

> **This repository is archived and no longer updated.**
> The blueprint continues, rewritten and covered by automated tests, as
> **[Bambu Printer Notifications](https://github.com/dabo53ck/bambu-printer-notifications-blueprint)**.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fdabo53ck%2Fbambu-printer-notifications-blueprint%2Fblob%2Fmain%2Fblueprints%2Fautomation%2Fdabo53ck%2Fbambu_printer_notifications.yaml)

## Why it moved

This began as a fork of
[HallyAus/homeassistant-bambu-blueprints](https://github.com/HallyAus/homeassistant-bambu-blueprints)
that took the snapshot when the printer stage leaves the printing phase instead
of at a progress percentage. It has since diverged too far to stay a fork:
different inputs, no TTS, its own tests and its own releases. It now lives in a
repository of its own, with issue forms, a changelog and CI.

## If you still use this blueprint

`bambu_print_notify.yaml` in this repository keeps working as before, but gets
no fixes. To switch, import the new blueprint and point your automation at it.
The [migration guide](https://github.com/dabo53ck/bambu-printer-notifications-blueprint/blob/main/docs/migration.md)
lists what changed, most notably that notifications now go to devices picked
from a device picker instead of a typed-in notify service.

Questions and bug reports go to the
[new repository](https://github.com/dabo53ck/bambu-printer-notifications-blueprint/issues).

## Credits and license

Original blueprint by [@HallyAus](https://github.com/HallyAus) —
[upstream repository](https://github.com/HallyAus/homeassistant-bambu-blueprints).

MIT, see [LICENSE](LICENSE). Upstream is released under
[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).
