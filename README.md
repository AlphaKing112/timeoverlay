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
  };
  ```

### 2. **Deploy the Overlay to GitHub Pages**
- Only upload the static files: `index.html`, `settings.html`, `config.js`, and any CSS/JS/images
- Do **not** upload the `overlay-remote/` directory or dev files
- Enable GitHub Pages in your repo settings (set source to `/root` or `/docs`)
- Your overlay will be live at `https://yourusername.github.io/yourrepo/`

### 3. **Configure the Overlay**
- Open `settings.html` from any device
- Enter a city (e.g. `Paris`) or your RTIRL key and save
- The overlay (`index.html`) will update automatically in OBS or any browser

### 4. **Add to OBS**
- Add a Browser Source in OBS
- Set the URL to your deployed `index.html` (e.g. `https://yourusername.github.io/yourrepo/index.html`)
- Set width/height as needed (e.g. 400x200)
- The overlay will update in real time as you change settings

---

## Cloudflare Worker Setup (for maintainers)

1. Install [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/get-started/):
   ```sh
   npm install -g wrangler
   wrangler login
   ```
2. Initialize and deploy the Worker:
   ```sh
   wrangler init overlay-remote --yes
   cd overlay-remote
   wrangler kv namespace create OVERLAY_KV
   wrangler kv namespace create OVERLAY_KV --preview
   # Add the KV binding info to wrangler.toml or wrangler.jsonc
   wrangler deploy
   ```
3. Use the Worker endpoint (e.g. `https://your-worker.workers.dev/overlay`) in your overlay and settings pages.

---

## Example API Key Config File

Create a file named `config.js` in your project root (or copy and rename `config.example.js`):

```js
window.OVERLAY_CONFIG = {
  openWeatherApiKey: "YOUR_OPENWEATHER_API_KEY",
  mapboxApiKey: "YOUR_MAPBOX_API_KEY"
};
```

**Do not commit your real API keys to public repositories.**

---

## How Local Time Works

The overlay uses the `timezone` offset provided by the OpenWeather API to calculate and display the correct local time for the selected city or RTIRL location. **No separate timezone API is needed.**

---

## License

MIT License

Copyright (c) 2024-present

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE. 