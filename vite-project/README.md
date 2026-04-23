# Weather App - Fully Functional Vue + Vite

Fully responsive weather web app using Vue 3 + Vite and OpenWeatherMap API. Search any location, view current conditions (temp, feels_like, humidity, wind, condition), geolocation support, localStorage persistence, error/loading handling.

## Features
- Search by city name (e.g. \"London\", \"New York\")
- Displays: Temperature (°C), condition icon/desc, humidity (%), wind (m/s), feels like
- Loading spinner, error messages (invalid location)
- Geolocation button (bonus)
- Remembers last search (localStorage, auto-load on reload)
- Responsive: Mobile/desktop (dark theme)
- Modern stack: Vue 3 Composition API

## Setup & Run (Local Development)
1. **API Key** (Required):
   - Sign up free at [OpenWeatherMap](https://openweathermap.org/api) → API keys → Current Weather Data (free tier).
   - Create `.env` file in root:
     ```
     VITE_API_KEY=your_api_key_here
     ```
   - Uses `import.meta.env.VITE_API_KEY` (Vite prefix required for client-side).

2. Install dependencies:
   ```
   npm install
   ```

3. Dev server (hot reload):
   ```
   npm run dev
   ```
   Open http://localhost:5173

4. Build for production:
   ```
   npm run build
   ```
   Preview: `npm run preview`

## Demo Flow
- Search \"London\" → Shows data.
- Invalid \"xyz123\" → \"Location not found\".
- Geolocation button → Uses browser location.
- Reload → Loads last city.
- Resize browser → Responsive card/input.

## Project Structure
```
vite-project/
├── src/components/WeatherApp.vue     # Main logic/UI
├── src/App.vue                       # Mounts WeatherApp
├── src/style.css                     # Responsive styles
├── src/main.js                       # Vue entry
├── index.html
├── vite.config.js
├── package.json
├── README.md
└── TODO.md                           # Progress tracker
```

## Known Limitations / Improvements
- No forecasts (current only).
- Rate limits: Free API ~60 calls/min.
- No auth/backend proxy (direct client API call; fine for local/demo).
- Icons: Emoji (add SVG library?).
- Deploy: Netlify/Vercel (set env var `VITE_API_KEY`).

## Technologies
- Frontend: Vue 3 (Composition), Vite, vanilla fetch, localStorage
- API: OpenWeatherMap (CORS-enabled)
- Responsive: CSS Grid/Flex, media queries
- No external deps needed beyond Vue/Vite

Enjoy your weather app! ☀️🌧️

