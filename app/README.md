# The Cloud Edition

A Victorian newspaper-style weather forecast web app.

## Quick Start

1. **Add your assets:**
   - Place your font files in `assets/fonts/`:
     - `ManufacturingConsent-Regular.woff2` (preferred)
     - `ManufacturingConsent-Regular.woff` (fallback)
     - `ManufacturingConsent-Regular.ttf` (fallback)
   - Place your paper texture in `assets/images/`:
     - `paper.png`

2. **Enable the paper texture:**
   
   In `index.html`, find the container style around line 470 and uncomment:
   ```js
   backgroundImage: 'url(./assets/images/paper.png)',
   backgroundRepeat: 'repeat',
   ```

3. **Deploy to GitHub Pages:**
   - Push this folder to a GitHub repository
   - Go to Settings → Pages
   - Set source to "Deploy from a branch"
   - Select `main` (or `master`) and `/ (root)`
   - Your site will be live at `https://[username].github.io/[repo-name]/`

## Project Structure

```
cloud-edition-web/
├── index.html              # Main app (single file, no build step)
├── README.md
└── assets/
    ├── fonts/
    │   └── ManufacturingConsent-Regular.woff2  (add this)
    └── images/
        └── paper.png                            (add this)
```

## Configuration

### Switching Providers

Near line 250 in `index.html`, you'll find:

```js
const WEATHER_PROVIDER = WeatherProviderType.OPEN_METEO;
const AI_PROVIDER = AIProviderType.DETERMINISTIC;
```

**Weather Providers:**
- `OPEN_METEO` - Free, no API key required (default)
- Add your own by implementing the provider interface

**AI Providers:**
- `DETERMINISTIC` - Template-based summaries, no API calls (default)
- `CLAUDE` - Uses Claude API for dynamic summaries (works in Claude.ai artifacts, requires API key for external hosting)

### Adding a New Weather Provider

1. Add to `WeatherProviderType`:
   ```js
   const WeatherProviderType = {
     OPEN_METEO: 'open-meteo',
     MY_PROVIDER: 'my-provider',
   };
   ```

2. Implement the provider:
   ```js
   const MyProvider = {
     async fetchWeather(lat, lon) {
       // Fetch from your API
       const response = await fetch(`https://api.example.com/weather?lat=${lat}&lon=${lon}`);
       const data = await response.json();
       return this.normalize(data);
     },
     
     normalize(data) {
       // Return normalized WeatherData structure
       return {
         current: { temp, feelsLike, humidity, wind, condition, conditionCode },
         daily: [{ date, high, low, condition, sunrise, sunset }],
         hourly: [{ date, temp, condition }],
         alerts: [],
       };
     },
   };
   ```

3. Add to factory:
   ```js
   function getWeatherProvider(type) {
     switch (type) {
       case WeatherProviderType.MY_PROVIDER:
         return MyProvider;
       // ...
     }
   }
   ```

### Adding a New AI Provider

Same pattern - implement `generateSummary(context)` and `generateTagline(placeName)`.

## Customization

### Colors

Find the CSS variables in the container style:
```js
'--text-color': '#1a1a1a',
'--paper-color': '#f5f1e8',
```

### Typography

The `styles` object (around line 310) contains all typography definitions. Modify sizes, weights, or font families there.

## Browser Support

Modern browsers with ES6+ support. Tested in:
- Chrome/Edge 90+
- Firefox 90+
- Safari 14+

## License

MIT
