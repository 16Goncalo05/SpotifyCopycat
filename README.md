# SpotifyCopycat
SpotifyCopycat


```markdown
#Music Assistant Setup with YouTube Bypass (PO Token) & Remote Access

This repository documents the deployment and configuration of **Music Assistant** in a Docker environment on an Ubuntu server. It addresses the recent strict YouTube Music playback restrictions using an automated **PO Token generator** and ensures secure remote access from anywhere (e.g., mobile data) using a **Tailscale VPN**.

---

#Architecture & Services Overview

* **Music Assistant Server:** A modern music streaming aggregator that unifies services like Spotify, YouTube Music, and local libraries.
* **YT Music PO Token Generator (`bgutil-ytdlp-pot-provider`):** A lightweight background API provider that constantly spins up Proof of Origin tokens to bypass YouTube's strict anti-bot systems.
* **Tailscale:** A secure, zero-config Mesh VPN network that links your phone to your home server securely without opening firewall ports on your ISP router.

---

#Step-by-Step Deployment Guide

### Step 1 - Deploy the PO Token Generator
To fix playback authentication crashes from YouTube Music, deploy the community-verified token generator. 

Running this container in `host` network mode ensures that the Music Assistant container can query the local loopback interface effortlessly on port `4416`:

```bash
docker run -d \
  --name yt-po-token-generator \
  --restart unless-stopped \
  --network host \
  brainicism/bgutil-ytdlp-pot-provider:latest




##Step 2 - Deploying the Music Assistant Server
# Create persistent storage folder
mkdir -p /home/goncalo/music-assistant

# Run the container
docker run -d \
  --name music-assistant \
  --restart unless-stopped \
  --privileged \
  -p 8095:8095 \
  -p 8096:8096 \
  -v /home/$name/music-assistant:/data \
  ghcr.io/music-assistant/server:stable




## Step 3 - Configure the YouTube Music Provider
Access the web panel at http://<YOUR_SERVER_IP_or_Localhost>:8095, go to Settings > Music Providers > Spotify:

Follow the authentication prompt to link your account, allowing Music Assistant to sync your custom playlists, followed artists, and albums seamlessly





##Remote Access Setup (Mobile & Out of Home)
To stream music or control your media center on mobile data without exposing your home infrastructure to malicious web scans, we route traffic through Tailscale.

#On the Server (Ubuntu):
If you don't have Tailscale deployed on the host yet, install and tie it to your account:

# Install Tailscale
curl -fsSL [https://tailscale.com/install.sh](https://tailscale.com/install.sh) | sh

# Spin up the node and follow the displayed web authentication link
sudo tailscale up



NOW:

On your Phone:
Download the official Tailscale app from the Google Play Store or Apple App Store.

Sign in with the exact same account provider used on your server.

Turn the VPN toggle switch ON.

Access Link:
When away from your home Wi-Fi network, target your private Tailscale address instead of your LAN IP:

Plaintext
[http://100.](http://100.)X.X.X:8095
Tip: Open this link in your mobile browser and select "Add to Home Screen" to install it as a lightweight Progressive Web App (PWA). To output the audio directly through your phone speakers or headphones, click the Player icon on the bottom right of the UI and select "Web Browser" / "This Browser".

