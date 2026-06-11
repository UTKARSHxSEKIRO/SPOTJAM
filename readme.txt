# Define the content for the how_to_use.txt file
readme_content = """# Spotify Jam Headless Audio Sync for Linux

An invisible, lightweight background daemon for Linux that allows Free Spotify accounts to sync and listen along with remote Spotify Jams. It intercepts track changes natively over D-Bus (MPRIS) and routes high-quality, ad-free audio streams headlessly in the background via yt-dlp and mpv.

---

### Features
- **100% Windowless & Invisible:** Runs quietly in the background without terminal windows, browser tabs, or external UIs.
- **Automatic Sync:** Hooks directly into Linux kernel system bus for 0ms delay tracking. Instantly swaps audio feeds on song skips.
- **Bypasses Premium Lockout:** Your local desktop client handles the Jam UI/lyrics, while this script handles the audio.
- **Built-in Ad-Blocker:** Bypasses all audio and visual advertisements completely.

---

### Dependencies Installation

#### Arch Linux / EndeavourOS:

```

```text
File successfully created!

```bash
sudo pacman -Sy --needed playerctl mpv yt-dlp python-pip lua

```

#### Ubuntu / Debian / Linux Mint:

```bash
sudo apt update
sudo apt install -y playerctl mpv python3-pip lua5.3
sudo wget [https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp) -O /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp

```

#### Fedora:

```bash
sudo dnf install -y playerctl mpv yt-dlp lua

```

---

### Installation & Setup

1. **Create the Script File:**
Save the core automation script to your home directory:
```bash
cat << 'EOF' > ~/headless-jam-sync.sh
#!/usr/bin/env bash

# Completely drop text output so it never crashes when terminal closes
exec >/dev/null 2>&1

mpv_pid=""

playerctl --player=spotify metadata --format "{{ title }} - {{ artist }}" --follow 2>/dev/null | while read -r track_info; do

    if [ -z "$track_info" ] || [ "$track_info" = " - " ]; then
        continue
    fi

    # Kill the previous song's player instance so audio never overlaps
    if [ -n "$mpv_pid" ] && kill -0 "$mpv_pid" 2>/dev/null; then
        kill "$mpv_pid"
    fi

    # Fetch the direct stream URL from yt-dlp and feed it cleanly to mpv
    stream_url=$(yt-dlp -f bestaudio "ytsearch:$track_info" -g 2>/dev/null)

    if [ -n "$stream_url" ]; then
        mpv --no-video "$stream_url" &
        mpv_pid=$!
    fi
done
EOF

```


2. **Make it Executable:**
```bash
chmod +x ~/headless-jam-sync.sh

```


3. **Configure Persistent Desktop Autostart:**
To make it launch silently in the background automatically every time you log into your desktop:
```bash
mkdir -p ~/.config/autostart/

cat << 'EOF' > ~/.config/autostart/jam-sync.desktop
[Desktop Entry]
Type=Application
Name=Spotify Jam Invisible Audio Sync
Comment=Forces audio sync over MPRIS D-Bus natively
Exec=/home/sekiroxutkarsh/headless-jam-sync.sh
Terminal=false
X-GNOME-Autostart-enabled=true
OS-Notify=false
EOF

```



---

### How To Use

1. Launch your regular official desktop Spotify application.
2. Join your friend's remote Spotify Jam link and choose **"Play from Aryanhuyr's speaker"** (or local device option).
3. The track details, timeline, and lyrics will update dynamically inside your Spotify UI.
4. The background daemon will instantly intercept the track updates and route the headless audio stream directly to your active headphones or speakers.

---

### Management & Troubleshooting Commands

* **To run it manually right away without logging out:**
```bash
setsid ~/headless-jam-sync.sh &

```


* **To stop the background sync script completely:**
```bash
pkill -f headless-jam-sync.sh && pkill mpv

```


* **To verify if the background loop process is actively running:**
```bash
ps aux | grep headless-jam-sync.sh

```



"""

# Write content to text file

with open("how_to_use.txt", "w") as file:
file.write(readme_content)
print("File successfully created!")

```
Your TXT file is ready
[file-tag: code-generated-file-0-1781194551665527480]

```text
# Spotify Jam Headless Audio Sync for Linux

An invisible, lightweight background daemon for Linux that allows Free Spotify accounts to sync and listen along with remote Spotify Jams. It intercepts track changes natively over D-Bus (MPRIS) and routes high-quality, ad-free audio streams headlessly in the background via yt-dlp and mpv.

---

### Features
- **100% Windowless & Invisible:** Runs quietly in the background without terminal windows, browser tabs, or external UIs.
- **Automatic Sync:** Hooks directly into Linux kernel system bus for 0ms delay tracking. Instantly swaps audio feeds on song skips.
- **Bypasses Premium Lockout:** Your local desktop client handles the Jam UI/lyrics, while this script handles the audio.
- **Built-in Ad-Blocker:** Bypasses all audio and visual advertisements completely.

---

### Dependencies Installation

#### Arch Linux / EndeavourOS:
```bash
sudo pacman -Sy --needed playerctl mpv yt-dlp python-pip lua

```

#### Ubuntu / Debian / Linux Mint:

```bash
sudo apt update
sudo apt install -y playerctl mpv python3-pip lua5.3
sudo wget [https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp) -O /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp

```

#### Fedora:

```bash
sudo dnf install -y playerctl mpv yt-dlp lua

```

---

### Installation & Setup

1. **Create the Script File:**
Save the core automation script to your home directory:
```bash
cat << 'EOF' > ~/headless-jam-sync.sh
#!/usr/bin/env bash

# Completely drop text output so it never crashes when terminal closes
exec >/dev/null 2>&1

mpv_pid=""

playerctl --player=spotify metadata --format "{{ title }} - {{ artist }}" --follow 2>/dev/null | while read -r track_info; do

    if [ -z "$track_info" ] || [ "$track_info" = " - " ]; then
        continue
    fi

    # Kill the previous song's player instance so audio never overlaps
    if [ -n "$mpv_pid" ] && kill -0 "$mpv_pid" 2>/dev/null; then
        kill "$mpv_pid"
    fi

    # Fetch the direct stream URL from yt-dlp and feed it cleanly to mpv
    stream_url=$(yt-dlp -f bestaudio "ytsearch:$track_info" -g 2>/dev/null)

    if [ -n "$stream_url" ]; then
        mpv --no-video "$stream_url" &
        mpv_pid=$!
    fi
done
EOF

```


2. **Make it Executable:**
```bash
chmod +x ~/headless-jam-sync.sh

```


3. **Configure Persistent Desktop Autostart:**
To make it launch silently in the background automatically every time you log into your desktop:
```bash
mkdir -p ~/.config/autostart/

cat << 'EOF' > ~/.config/autostart/jam-sync.desktop
[Desktop Entry]
Type=Application
Name=Spotify Jam Invisible Audio Sync
Comment=Forces audio sync over MPRIS D-Bus natively
Exec=/home/sekiroxutkarsh/headless-jam-sync.sh
Terminal=false
X-GNOME-Autostart-enabled=true
OS-Notify=false
EOF

```



---

### How To Use

1. Launch your regular official desktop Spotify application.
2. Join your friend's remote Spotify Jam link and choose **"Play from Aryanhuyr's speaker"** (or local device option).
3. The track details, timeline, and lyrics will update dynamically inside your Spotify UI.
4. The background daemon will instantly intercept the track updates and route the headless audio stream directly to your active headphones or speakers.

---

### Management & Troubleshooting Commands

* **To run it manually right away without logging out:**
```bash
setsid ~/headless-jam-sync.sh &

```


* **To stop the background music sync completely:**
```bash
pkill -f headless-jam-sync.sh && pkill mpv

```


* **To verify if the background loop process is actively running:**
```bash
ps aux | grep headless-jam-sync.sh

```



```

```
