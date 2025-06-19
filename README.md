# Bash Items

A collection of bash scripts for automating MikroTik RouterOS maintenance tasks.

---

## Scripts Overview

### `versionTest`
- Designed for use as a cron job.
- Downloads new `.npk` package files from storage.
- Packages are later uploaded using Seafile.

### `rosbackup`
- Connects to RouterOS devices via SSH.
- Fetches backup files from each device.

### `massupgrade`
- Uses SSH to connect to RouterOS devices.
- Triggers the built-in **auto-upgrade** functionality on each device.

### `massupgraderepo`
- Runs SSH across a specified IP range.
- Uses the `fetch` command to download necessary `.npk` packages from a specified repository.

---

Each script is intended to streamline RouterOS device management in bulk environments.
