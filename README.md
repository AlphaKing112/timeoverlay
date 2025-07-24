# TimeOverlay Remote-Controlled OBS Overlay

A real-time, remotely controlled overlay for OBS (or any stream) that displays your current time, location, weather, and more. Control the overlay from any device using a simple settings page, powered by Cloudflare Workers KV.

---

## Features
- Real-time location and weather overlay for OBS or browser
- Remote control: update city or RTIRL key from any device
- Supports both manual city input and RealtimeIRL location
- Beautiful, mobile-friendly UI
- Free and open source (MIT License)

---

## Quick Start

### 1. **Clone the Repo and Set Up API Keys**
- Clone this repo (or your fork)
- Copy `config.example.js` to `config.js`:
  ```sh
  cp config.example.js config.js
  ```
- Edit `config.js` and input your own API keys for OpenWeather and Mapbox:
  ```js
  window.OVERLAY_CONFIG = {
    openWeatherApiKey: "YOUR_OPENWEATHER_API_KEY",
    mapboxApiKey: "YOUR_MAPBOX_API_KEY"
    workerEndpoint: "WORKER_ENDPOINT"
  };
  ```

### 2. ## How to Deploy Your Own Overlay (Backend + Frontend) on Cloudflare

You can have your own private overlay by deploying both the backend (Worker) and frontend (Pages) on Cloudflare:

1. **Deploy the Worker (Backend API)**
   - Follow the steps in “How to Set Up Your Own Worker Endpoint” above.
   - This will give you a URL like `https://your-worker.workers.dev/overlay`.

2. **Deploy the Overlay UI (Frontend) with Cloudflare Pages**
   - Push your static files (`index.html`, `settings.html`, etc.) to a GitHub repo.
   - Create a new Cloudflare Pages project and connect your repo.
   - Use the default settings for a static site (no build command, output directory is `.`).
   - Your site will be live at `https://your-project.pages.dev/`.

3. **Connect the Frontend to Your Worker**
   - In your deployed site’s `config.js`, set:
     ```js
     window.OVERLAY_CONFIG = {
       openWeatherApiKey: "",
       mapboxApiKey: "",
       workerEndpoint: "https://your-worker.workers.dev/overlay"
     };
     ```
   - Now your overlay UI will use your own backend for all settings and data.

This approach gives each user their own backend and frontend, fully under their control!

### 3. **Configure the Overlay**
- Open `settings.html` from any device
- Enter a city (e.g. `Paris`) or your RTIRL key and save
- The overlay (`index.html`) will update automatically in OBS or any browser

### Where to Get Your RTIRL Key

To use live location tracking, you need a RealtimeIRL (RTIRL) key:
- Go to [https://realtime.irl.com/](https://realtime.irl.com/) and sign up or log in.
- Create a new "pull key" in your RTIRL dashboard.
- Copy your pull key and enter it in the settings page.

This key allows your overlay to receive your real-time location from the RTIRL app or service.

### 4. **Add to OBS**
- Add a Browser Source in OBS
- Set the URL to your deployed `

## How to Set Up Your Own Worker Endpoint

You can deploy your own Cloudflare Worker to store and serve your overlay settings. This gives you full control and privacy for your overlay data.

### Steps:

1. **Create a Cloudflare Account**
   - Go to [https://dash.cloudflare.com/sign-up](https://dash.cloudflare.com/sign-up) and sign up (free).

2. **Install Wrangler CLI**
   - Install Node.js if you don't have it: [https://nodejs.org/](https://nodejs.org/)
   - Open your terminal and run:
     ```sh
     npm install -g wrangler
     wrangler login
     ```

3. **Initialize and Deploy the Worker**
   - Clone or copy the `overlay-remote` Worker project from this repo (or use your own):
     ```sh
     wrangler init overlay-remote --yes
     cd overlay-remote
     wrangler kv namespace create OVERLAY_KV
     wrangler kv namespace create OVERLAY_KV --preview
     # Add the KV binding info to wrangler.toml or wrangler.jsonc
     wrangler deploy
     ```
   - After deployment, you'll get a URL like `https://your-worker.workers.dev/overlay`.

4. **Copy Your Worker Endpoint**
   - Use this URL as the `workerEndpoint` in your `config.js`:
     ```js
     window.OVERLAY_CONFIG = {
       ...
       workerEndpoint: "https://your-worker.workers.dev/overlay"
     };
     ```

5. **(Optional) Add a Custom Domain**
   - In the Cloudflare dashboard, go to your Worker > Triggers > Custom Domains.
   - Add your domain or subdomain and follow the DNS instructions.

Now your overlay and settings pages will use your own Worker endpoint for all data storage and retrieval!


## Example API Key Config File

-Create a file named `config.js` in your project root (or copy and rename `