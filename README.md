# JTV

JTV is a Jellyfin-style media server for **Movies**, **TV Shows**, and **Live TV**. **Each JTV account has its own Xtream Codes login.** Do not share one Xtream user across several JTV accounts — that causes **404** and **permission denied** on streams. The web UI includes posters, search, favorites, continue watching, and a TiviMate-style live TV guide.

The **JTV player app will be on Google Play soon.** This repository describes the product. It does not include the player application.

## What JTV does

- **Home** — continue watching, favorites, and library shelves
- **Movies** — poster grid, details, playback
- **TV Shows** — series, seasons, and episodes
- **Live TV** — channels plus an EPG-style **Guide**
- **Search** — movies, series, and live channels
- **Favorites** — star titles and live channels
- **Dashboard** — admins manage users, assign a **separate Xtream Codes login to each account** (required to avoid 404 / permission denied), watch host CPU/memory, and download the Android / Android TV / Xbox apps from buttons at the bottom of the page
- **Playback** — remux when the client can handle it, HLS transcode as fallback (GPU NVENC when available)

## Screenshots

Home, with library shortcuts and favorites:

![JTV Home](screenshots/home.png)

Movies, with categories and poster rows:

![JTV Movies](screenshots/movies.jpg)

Admin dashboard, with accounts, Xtream assignment, and app download buttons:

![JTV Dashboard](screenshots/dashboard.png)

JTV is a player-and-server for a **server you run**. It does not include Netflix, Disney+, or any other streaming subscription.

## How it is put together

```
Your Ubuntu box (JTV server, port 18096)
        │
        ├── browser:  http://YOUR_SERVER:18096
        └── player:   Android / TV / Xbox (Google Play soon)
                      connect to the same server, then sign in
```

The player is a remote client, like Jellyfin’s apps. It asks for the server address, you sign in, then you browse and play. It does **not** host JTV on the phone or TV.

Default web admin login on a new install is `admin` / `changeme`. Change that immediately.

## Run the server (Ubuntu + Docker)

Host port **18096** is used so it does not collide with Jellyfin on 8096.

```bash
sudo apt update
sudo apt install -y docker.io docker-compose-v2
sudo usermod -aG docker "$USER"
# log out and back in so docker works without sudo

# copy your JTV server package onto the machine, then:
cd ~/jtv
chmod +x install-ubuntu.sh
./install-ubuntu.sh
```

If the firewall is on: `sudo ufw allow 18096/tcp`. Library data lives in a Docker volume. Open `http://YOUR_SERVER:18096`, sign in, and in the Dashboard give **each user a separate Xtream Codes username and password**. Sharing one Xtream account between JTV users leads to **404** and **permission denied** errors.

## Xtream Codes (one login per JTV user)

JTV talks to your provider with the Xtream credentials stored on **that** JTV account.

- Assign a **unique** Xtream user to every JTV user in the Dashboard.
- Two people signed into JTV at once with the **same** Xtream code will hit connection limits and get **404** or **permission denied**.
- Playback, Live TV, and the guide all use that per-account Xtream login, so a missing or shared code shows up as those errors rather than a JTV bug.

## Player (version 2.0)

The JTV player is a remote client. It does **not** host the library. One APK covers phones, tablets, Android TV / Google TV / Fire TV, and Xbox (Developer Mode sideload).

**Latest player behavior**

- Connect first: enter `http://YOUR_SERVER:18096` (no example IP). `localhost` is rejected because that is the phone, TV, or Xbox.
- Then sign in with your JTV username and password. **Remember me** keeps the session. **Change server** returns to Connect.
- Browse **Home**, **Movies**, **TV Shows**, **Live TV**, **Guide**, and **Search** — the same libraries as the web app.
- Favorites, continue watching, and a TiviMate-style live guide.
- Playback uses ExoPlayer (Media3): remux when the device can handle it, HLS transcode as fallback.
- Touch on phones; D-pad / Leanback on TV; gamepad on Xbox.

The player app **will be available on Google Play soon.** This repository still does not include the player source.

Until then, the admin Dashboard has **Android**, **Android TV**, and **Xbox** download buttons at the bottom of the page. Xbox sideloads the Android TV APK in Developer Mode.

Keep the device on the same network as the server unless you expose JTV yourself.

## Privacy

See [PRIVACY.md](PRIVACY.md). JTV is self-hosted. The upcoming Play Store player only talks to the server address you enter.

## Status

- Server and web UI: in use on Ubuntu (Docker)
- Player app **2.0**: Android / Android TV / Xbox sideload from Dashboard downloads; **coming to Google Play soon** (source not in this repository)
