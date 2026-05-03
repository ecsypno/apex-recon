![alt text](gfx/apex%20recon%20banner.png)

Installation instructions for [Apex Recon](https://ecsypno.com/pages/codename-rkn):

* [Docker installation](#docker-installation)
    * [Updating](#updating)
    * [Caution!](#caution)
* [Automated installation](#automated-installation) -- for Linux. 
* [Manual installation](#manual-installation) -- for Linux.
* [Dependencies for headless environments or WSL](#dependencies-for-headless-environments-or-wsl)

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

## License

Copyright 2026 [Ecsypno](https://ecsypno.com/).

All rights reserved.
