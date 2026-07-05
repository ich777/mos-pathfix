# MOS Path Fix

mos-pathfix provides a **MOS plugin** that rewrites the legacy `/ich777/`
path prefix to `/mos-nas/` across installed plugin scripts, init scripts and
plugin `functions` files.

---

## Overview

The plugin ships a small script at `/usr/bin/plugins/pathfix` that runs the
following replacements:

- `/usr/local/bin`
- `/etc/init.d`
- `/boot/optional/plugins/*/*/functions`

In each location every file still containing `/ich777/` is rewritten to
`/mos-nas/`.

The `functions` file runs the script automatically:

- `install` – on plugin installation
- `mos_start` – on every boot

The plugin page provides a button to trigger the same replacement again on
demand.

---

## Build & Automation

This repository includes a **GitHub Actions workflow** used to build and
package the plugin for MOS. The build produces a `.deb` artifact that can be
installed through the MOS Hub.

---

## Licensing

The contents of this repository are licensed under **GPL-3.0**.
