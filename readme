SpotJam 🎧

An ultra-lightweight, zero-UI background daemon for Linux that allows Free Spotify accounts to sync and listen along in remote Spotify Jams completely ad-free.

SpotJam hooks directly into the Linux D-Bus system to watch your local Spotify client in real-time. The moment a song changes inside your friend's Jam session, SpotJam intercept the metadata, searches for the high-quality raw audio track, and routes a direct stream headlessly to your headphones.

🚀 Key Features

⚡ Zero-Delay Auto-Sync: Intercepts track changes natively at the kernel level with 0ms delay.

🕵️ 100% Invisible Background Execution: Runs cleanly as a detached background session. No open terminals, browser tabs, or bulky electron wrappers required.

🚫 Premium Lockout Bypass: Keep your Free account! Let your official Spotify app handle the UI, lyrics, and social Jam lobby, while this script handles the audio delivery.

🔇 Automated Ad-Blocker: Direct streams bypass all visual and audio advertisements natively.

🔒 System Architecture

                               ┌────────────────────────┐
                               │  Your Spotify Client   │
                               │  (Joined to Host Jam)  │
                               └───────────┬────────────┘
                                           │
                        D-Bus (MPRIS)      ▼ Track Metadata Update
                     ┌────────────────────────────────────────┐
                     │         headless-jam-sync.sh           │
                     └─────────────────────┬──────────────────┘
                                           │
                      Queries Stream       ▼ Resolves Direct Link
                     ┌────────────────────────────────────────┐
                     │          yt-dlp (Search Engine)        │
                     └─────────────────────┬──────────────────┘
                                           │
                     Pipes Raw Audio URL   ▼
                     ┌────────────────────────────────────────┐
                     │           mpv (Headless Player)        │
                     └─────────────────────┬──────────────────┘
                                           │
                                           ▼ 🔊 Direct To Headphones


🛠️ Dependency Installation

You must install playerctl (to monitor Spotify metadata), mpv (to play raw audio), and yt-dlp (to fetch raw streams).

Arch Linux / EndeavourOS

sudo pacman -Sy --needed playerctl mpv yt-dlp python-pip lua


Ubuntu / Debian / Linux Mint

sudo apt update
sudo apt install -y playerctl mpv python3-pip lua5.3
sudo wget [https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp](https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp) -O /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp


Fedora

sudo dnf install -y playerctl mpv yt-dlp lua


📦 Setup & Installation

Follow these three simple steps to lock the daemon into your system:

1. Create the Script

Save the silent background script into your user directory:

cat << 'EOF' > ~/headless-jam-sync.sh
#!/usr/bin/env bash

# Completely mute text output so it never crashes when the terminal is closed
exec >/dev/null 2>&1

mpv_pid=""

playerctl --player=spotify metadata --format "{{ title }} - {{ artist }}" --follow 2>/dev/null | while read -r track_info; do
    
    if [ -z "$track_info" ] || [ "$track_info" = " - " ]; then
        continue
    fi
    
    # Clean kill of the previous player instance to prevent overlapping streams
    if [ -n "$mpv_pid" ] && kill -0 "$mpv_pid" 2>/dev/null; then
        kill "$mpv_pid"
    fi

    # Query yt-dlp for the direct audio stream and hand it directly to mpv
    stream_url=$(yt-dlp -f bestaudio "ytsearch:$track_info" -g 2>/dev/null)
    
    if [ -n "$stream_url" ]; then
        mpv --no-video "$stream_url" &
        mpv_pid=$!
    fi
done
EOF


2. Make the Script Executable

chmod +x ~/headless-jam-sync.sh


3. Register as an Autostart Service

Create an entry so your desktop environment starts SpotJam invisibly the second you log in:

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


🎮 How To Use

Start Spotify: Open your official Spotify desktop app.

Join the Jam: Open the remote Jam link shared by your friend and select the local playback option.

Enjoy the Music: SpotJam automatically takes over from the very first track. Close the terminal window completely—the script remains active in your background.

⚙️ Control & Troubleshooting Commands

Since the application runs silently without a GUI, use these simple terminal shortcuts to manage it:

Start the sync process manually right now (without rebooting):

setsid ~/headless-jam-sync.sh &


Verify if the script is currently running in your background:

ps aux | grep headless-jam-sync.sh


Kill the sync process and turn off background playback:

pkill -f headless-jam-sync.sh && pkill mpv


Developed by UTKARSHxSEKIRO
