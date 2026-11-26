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

## Remote access with Tailscale

Install [Tailscale](https://tailscale.com/docs/install) on the machine running the Synker hub. Find its Tailscale IPv4 address in [Machines](https://login.tailscale.com/admin/machines). In Synker, use `http://<hub-Tailscale-IP>:8484`, for example `http://100.101.102.103:8484`. Keep port 8484 private on your internet router.

### Switch, Switch Lite and Switch OLED

1. Install [tailscale-nx](https://github.com/UnderwaterOverground/tailscale-nx#install) on a Switch running Atmosphère. Open it in Ultrahand and scan its QR code to join the same Tailscale network as the hub.
2. Install the `Synker.nro` from [Synker 3.3.5](https://github.com/tutur1307/Synker-site/releases/tag/v3.3.5) or later. The same file is used on all three Switch models.
3. Launch Synker from the Homebrew Menu. In **Settings → Hub address**, enter the hub's Tailscale URL. If you have already paired with that hub, changing its address keeps the pairing. **Settings → App version** checks whether the hub responds before you sync.

4. To sync in the background as well, use [Synker 3.4.1](https://github.com/tutur1307/Synker-site/releases/tag/v3.4.1) or later and turn on **Settings → Background sync**. Then restart the console once. Background sync runs as a separate sysmodule, which tailscale-nx only puts on your Tailscale network when it is listed in tailscale-nx's settings. Synker adds it to `/config/tailscale-nx/config.ini` for you, and tailscale-nx reads that file when the console starts. If you install tailscale-nx after turning on background sync, open Synker once, then restart the console.

If background sync cannot reach the hub, Synker's **Activity** tab shows "Cannot reach the hub".

### 3DS, Android and Windows

- **3DS:** Connect the console to a Wi-Fi router that can forward its clients' traffic to Tailscale, then enter the hub's Tailscale URL in Synker. For example, use a compatible [GL.iNet router with IP Masquerading](https://docs.gl-inet.com/router/en/4/interface_guide/tailscale/#ip-masquerading). Set its Wi-Fi to 2.4 GHz with WPA2-AES for the [3DS](https://en-americas-support.nintendo.com/app/answers/detail/a_id/498/~/compatible-wireless-modes-and-wireless-security-types).
- **Android and Windows:** Install the [Tailscale app](https://tailscale.com/download) on the device, join the same Tailscale network as the hub, and use the hub's Tailscale URL in Synker. You can test the connection in a browser at `http://<hub-Tailscale-IP>:8484/api/health`.

### Update the hub

From the folder with `docker-compose.yml`, run:

```sh
docker compose pull
docker compose up -d
```
