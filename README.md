# Bangor Water Level

A real-time web application monitoring river level, rainfall, weather, and tide conditions for Bangor salmon fishing in County Down, Northern Ireland.

## Overview

This application aggregates data from multiple sources:

- **EPA Hydronet** - River level data for Bangor station 33008
- **Met Éireann** - Weather forecasts for Owenninney and Altnabrocky catchments
- **Marine Institute** - Ballyglass tidal predictions (corrected for Blacksod Bay)

Data is displayed via interactive charts and a real-time status dashboard. The application runs entirely in the browser with no backend server required.

## Features

- **Current River Conditions** - EPA level to TBM, adjusted gauge level, and river trend analysis
- **River Level Chart** - Interactive 6h, 12h, 24h, 48h, 3d, and 7d views
- **Rainfall Tracking** - Historical and forecast rainfall with 14-day archive
- **Weather Forecast** - 7-day weather conditions with wind, pressure, and temperature
- **Tide Predictions** - Hourly tide curve and high/low tide times (corrected for local conditions)
- **Live Status Indicators** - Real-time status for EPA, Met Éireann, and Marine Institute data sources

## Data Sources

### EPA Hydronet River Data
- **Source:** EPA Hydronet public API (ZIP file with CSV data)
- **Station:** Bangor (33008)
- **Data:** Absolute level to TBM (m) and relative stage level
- **Update Frequency:** EPA typically updates 2-3 times daily
- **URL:** `https://epawebapp.epa.ie/Hydronet/output/internet/stations/CAS/33008/S/3_months.zip`

### Met Éireann Weather
- **Source:** Met Éireann locationforecast API (HTTPS endpoint)
- **Locations:** 
  - Owenninney (54.174°N, -9.557°W)
  - Altnabrocky (54.061°N, -9.617°W)
- **Data:** Temperature, wind, pressure, cloud cover, precipitation
- **Update Frequency:** Multiple forecasts per day
- **URL:** `https://openaccess.pf.api.met.ie/metno-wdb2ts/locationforecast?lat=<lat>;long=<long>`

### Marine Institute Tides
- **Source:** Marine Institute ERDDAP server
- **Station:** Ballyglass
- **Data:** High/low tide times and water level predictions
- **Corrections:** +1 hour adjustment for observed Blacksod Bay timing, converted to Irish local time

## Technical Stack

- **Frontend:** Vanilla JavaScript (ES6+), no frameworks
- **Charting:** Chart.js 4.4.7
- **Compression:** JSZip 3.10.1 (for EPA ZIP parsing)
- **Storage:** Browser localStorage (rainfall snapshot history)
- **Hosting:** GitHub Pages with CORS-enabled API endpoints

## Local Data Storage

The application caches rainfall forecasts in browser localStorage:
- **Key:** `bangorWaterLevel.rainfallSnapshots`
- **Max Age:** 14 days
- **Max Items:** 5000 snapshots
- **Purpose:** Build historical rainfall lookback over time

## Installation & Development

### Local Setup

1. Clone this repository:
   ```bash
   git clone https://github.com/davyamadaos/bangor-water-level-v7-.git
   cd bangor-water-level-v7-
   ```

2. Serve locally (requires a local HTTP server):
   ```bash
   python3 -m http.server 8000
   # or
   npx http-server
   ```

3. Open in browser:
   ```
   http://localhost:8000
   ```

### GitHub Pages Deployment

This repository is configured for GitHub Pages:
1. Push to main branch
2. Enable GitHub Pages in repository settings (Settings → Pages → Source: main branch)
3. Access at: `https://davyamadaos.github.io/bangor-water-level-v7-/`

## Troubleshooting

### "Data feeds not loading"
- **Check browser console** (F12) for CORS errors or network issues
- **Verify internet connection**
- **Check data source status badges** at top of page (EPA/Met Éireann/Marine Institute)
- **Refresh data** using the "Refresh" button
- **Wait for API responses** - Initial load may take 5-10 seconds
- **Ensure .nojekyll file is deployed** to prevent Jekyll interference

### "EPA Level Unavailable"
- EPA may be experiencing downtime or API issues
- Historical data displayed if previously loaded
- EPA updates typically 2-3 times daily
- Check EPA status: https://www.epa.ie/

### "Weather/Rainfall Unavailable"
- Met Éireann API may be temporarily unavailable
- HTTPS endpoint used (HTTP redirected to HTTPS)
- Check Met Éireann status: https://www.met.ie/

### "Tide Data Unavailable"
- Marine Institute ERDDAP server may be down
- Check Marine Institute status: https://www.marine.ie/

### "CORS or Mixed Content Errors"
- Application uses HTTPS endpoints where possible
- Check browser console for specific CORS errors
- Ensure .nojekyll file is deployed

## Browser Compatibility

- Modern browsers with ES6+ support
- Requires localStorage support
- Chart.js requires canvas support
- Tested on: Chrome 90+, Firefox 88+, Safari 14+, Edge 90+

## Files Structure

```
bangor-water-level-v7-/
├── .nojekyll            # Prevent Jekyll processing
├── 404.html             # GitHub Pages routing
├── index.html           # Main HTML document
├── app.js              # Core application logic
├── styles.css          # Styling and responsive layout
├── README.md           # Documentation
└── LICENSE             # MIT License
```

## Performance

- **Initial Load:** ~2-5 seconds (depends on EPA ZIP size and API response times)
- **Update Cycle:** 15 minutes (configurable in `CONFIG.refreshMs`)
- **Storage:** ~5MB for 5000 rainfall snapshots in localStorage
- **Chart Rendering:** ~100-200ms per chart update

## Attribution & Licenses

- **EPA Hydronet Data:** Irish Environmental Protection Agency
- **Met Éireann Weather:** Met Éireann (Creative Commons Attribution 4.0)
- **Marine Institute Tides:** Marine Institute (Creative Commons Attribution 4.0)
- **Chart.js:** MIT License (https://www.chartjs.org/)
- **JSZip:** MIT or GPLv3 dual license (https://stuk.github.io/jszip/)

## License

MIT License - See LICENSE file for details

---

**Last Updated:** June 2026  
**Version:** 7.0
