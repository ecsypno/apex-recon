![alt text](gfx/apex%20recon%20banner.png)

Installation instructions for [Apex Recon](https://ecsypno.com/pages/codename-rkn):

* [Docker installation](#docker-installation)
    * [Updating](#updating)
    * [Caution!](#caution)
* [Automated installation](#automated-installation) -- for Linux. 
* [Manual installation](#manual-installation) -- for Linux.
* [Dependencies for headless environments or WSL](#dependencies-for-headless-environments-or-wsl)
* [Environment variables](#environment-variables) -- for ops, air-gapped, multi-volume layouts.

## Docker installation

```bash
mkdir apex-recon && cd apex-recon
curl -sSL https://compose.apex-recon.sh > docker-compose.yml

docker compose pull
docker compose up -d # Start the services.

docker exec -it apex-recon-app-1 bash # Connect to the container.
```
You can now run Apex Recon by using the executables under the `apex-recon-v*/bin` directory.

1. For a CLI scan you can run `bin/apex URL`.
2. You can use Apex Recon Pro by running `bin/apex_pro`.

For more information please consult the [documentation](https://documentation.ecsypno.com/rkn/).

### Updating

You can run `./setup.sh` when a new version is released to install it automatically.

### Caution!

Use `docker compose stop` to stop the container, and `docker compose start` to start it again.

**DO NOT** use `docker compose up` nor `docker compose down`, as they will delete all filesystem
data.

## Automated installation

To install run the following command in a terminal of your choice:

```bash
bash -c "$(curl -sSL https://apex-recon.sh)"
```

You can now run Spectre Scan by using the executables under the `spectre-scan-v*/bin` directory.

1. For a CLI scan you can run `bin/apex URL`.
2. You can use Spectre Scan Pro by running `bin/apex_pro`

For more information please consult the [documentation](https://documentation.ecsypno.com/rkn/).

## Manual installation

1. Download the [latest package](https://github.com/ecsypno/apex-recon/releases).
2. Extract.
3. Activate: `bin/apex_activate [LICENSE_KEY]`

You can now run Spectre Scan by using the executables under the `spectre-scan-v*/bin` directory.

1. For a CLI scan you can run `bin/apex URL`.
2. You can use Spectre Scan Pro by running `bin/apex_pro`

For more information please consult the [documentation](https://documentation.ecsypno.com/rkn/).

## Dependencies for headless environments or WSL

For minimal environments such as headless servers or the Windows Subsystem for Linux, please first run:

```
sudo apt-get update
sudo apt-get install -y libgconf-2-4 libatk1.0-0 libatk-bridge2.0-0 \
  libgdk-pixbuf2.0-0 libgtk-3-0 libgbm-dev libnss3-dev libxss-dev libasound2
```

## Environment variables

These can be exported **before** running the installer (or before the first
invocation of any `bin/apex*` executable) to override defaults. Most users
won't need any of them — they exist for ops folks running on shared boxes,
behind proxies, with separate disks, or in air-gapped environments.

### Home directory

| Variable | Default | What it does |
|---|---|---|
| `APEX_HOME` | `~/.apex` | Single root for **all** Apex Recon user-scoped state — the encrypted `license` blob and plaintext `license.key`, scan reports, snapshots, engine logs, the embedded PostgreSQL data directory, and the `latest_version` stamp written by the auto-update check. Override this to put everything on a different volume (e.g. `APEX_HOME=/mnt/scratch/apex`). |

### Logs

| Variable | Default | What it does |
|---|---|---|
| `APEX_ENGINE_LOG_DIR` | `$APEX_HOME/logs/engine` | Engine-side run logs. |
| `APEX_PRO_LOG_DIR` | `$APEX_HOME/logs/pro` | Apex Recon Pro (Rails) + bundled PostgreSQL logs. |

### Apex Recon Pro database (embedded PostgreSQL)

| Variable | Default | What it does |
|---|---|---|
| `APEX_PRO_DB_DIR` | `$APEX_HOME/pro/db` | PostgreSQL cluster data dir (`initdb -D`). Move this to a faster / larger volume for big scans. |
| `APEX_PRO_DB_SOCKET_DIR` | `$APEX_HOME/pro/run` | Unix socket dir Pro connects on. |
| `APEX_PRO_DB_NAME` | `apex_pro` | Database name created on first start. |
| `APEX_PRO_PG_PASSWD` | *(none)* | Password for the Pro DB user. The Rails app reads this on startup; required when running `assets:precompile` or any production boot. |

### Engine

| Variable | Default | What it does |
|---|---|---|
| `SPECTRE_CHECK_SERVER` | `http://checks.ecsypno.com` | URL of the SSRF check server (Apex Recon embeds the SCNR engine, so the engine-side env var still uses the `SPECTRE_` prefix). Override it for air-gapped installs running their own check server — see the [air-gapped guide](https://documentation.ecsypno.com/rkn/how-to/run-air-gapped.html). |
| `SPECTRE_ENGINE_PROFILE` | *(unset)* | Set to anything truthy to enable the engine's profiling output (verbose, dev/debug only). |

### Networking

| Variable | Default | What it does |
|---|---|---|
| `APEX_PROXY` | *(unset)* | Forwarded as `HTTP_PROXY` / `http_proxy` for the build's outbound calls (release feed lookup, dependency fetches, license server pings). |
| `CURL_CA_BUNDLE` / `SSL_CERT_FILE` | bundled `etc/ssl/ca/cacert.pem` | Override if you need to trust an enterprise / corporate CA bundle. |

### Standard

| Variable | What it does |
|---|---|
| `TMPDIR` | Where engine snapshot/work dirs are created (`Dir.mktmpdir`). Set to a large volume if your runs hit "no space left on device". |
| `TZ` | Falls into `Setting.detect_system_timezone` chain (after `Rails.application.config.time_zone` / `/etc/timezone` / `/etc/localtime`) when nothing else resolves. |

## License

Copyright 2026 [Ecsypno](https://ecsypno.com/).

All rights reserved.
