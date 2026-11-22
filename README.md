# Synker

Synker keeps game saves in sync between modded consoles, Android handhelds and PC emulators. The hub stores your saves and their version history on a machine on your home network.

## Install with Docker

The hub image supports `linux/amd64` and `linux/arm64`. Use a NAS, home server or PC that can run Linux containers.

### Docker Compose

1. Create a folder named `synker`. Open [docker-compose.yml](docker-compose.yml) and use GitHub’s **Download raw file** button to save it in that folder.
2. Open a terminal in the folder and run:

   ```sh
   docker compose up -d
   ```

### Synology Container Manager

1. In **File Station**, create `docker/synker` and put [docker-compose.yml](docker-compose.yml) in it.
2. In **Container Manager → Project → Create**, name the project `synker` and select `docker/synker` as its path.
3. Choose **Use existing docker-compose.yml** and finish creating the project.

### Open the hub

1. On the same network, go to `http://<hub-IP>:8484` in a browser. For example, if your NAS is at `192.168.1.20`, open `http://192.168.1.20:8484`.
2. Set the web interface password and follow the setup wizard.
3. Open **Devices → Add a device** to download the app for your device and pair it. The apps are also available under [Releases](https://github.com/tutur1307/Synker-site/releases).

The database and save archives are in the `data` folder next to `docker-compose.yml`. Back up the whole folder. Keep port `8484` on your local network.

If the interface doesn’t open, check that the container is running and that your device can reach port `8484` on the hub. A working hub also returns `"status":"ok"` at `http://<hub-IP>:8484/api/health`.

### Update the hub

From the folder with `docker-compose.yml`, run:

```sh
docker compose pull
docker compose up -d
```
