# Map Integration - Leaflet

## Overview

The Quick Help Map uses Leaflet with OpenStreetMap tiles for displaying task locations.

---

## Why Leaflet?

| Option | Cost | Pros | Cons |
|--------|------|------|------|
| **Leaflet** ✅ | Free | No API keys, lightweight, great React support | Less polished than Google |
| Google Maps | $7/1000 loads | Most familiar, best imagery | Requires billing |
| Mapbox | $5/1000 loads | Beautiful styling, 3D | Complex setup |

**Decision**: Leaflet is perfect for MVP - zero cost, quick setup, easy migration later if needed.

---

## Installation

```bash
npm install leaflet react-leaflet @types/leaflet
```

---

## Basic Usage

```tsx
import { MapContainer, TileLayer, Marker, Popup } from 'react-leaflet'
import 'leaflet/dist/leaflet.css'

<MapContainer center={[56.9496, 24.1052]} zoom={13}>
  <TileLayer
    url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
  />
  <Marker position={[56.9496, 24.1052]}>
    <Popup>Task here!</Popup>
  </Marker>
</MapContainer>
```

---

## Custom Markers

We use SVG-based markers with price labels:

```tsx
const createPriceMarker = (price: number) => {
  const color = price < 30 ? '#22c55e' : price < 60 ? '#3b82f6' : '#8b5cf6'
  
  return new Icon({
    iconUrl: `data:image/svg+xml;base64,${btoa(`
      <svg xmlns="http://www.w3.org/2000/svg" width="50" height="30">
        <rect width="50" height="30" rx="15" fill="${color}"/>
        <text x="25" y="20" font-size="12" text-anchor="middle" fill="white">€${price}</text>
      </svg>
    `)}`,
    iconSize: [50, 30],
    iconAnchor: [25, 30],
    popupAnchor: [0, -30]
  })
}
```

---

## Color Coding

| Budget | Color | Meaning |
|--------|-------|--------|
| < €30 | Green | Budget tasks |
| €30-60 | Blue | Standard tasks |
| > €60 | Purple | Premium tasks |

---

## Map Configuration

- **Center**: Riga, Latvia (56.9496°N, 24.1052°E)
- **Default Zoom**: 13 (neighborhood level)
- **Tile Provider**: OpenStreetMap (free, no API key)

---

## Future Improvements

1. **Clustering** - Group nearby markers
2. **User location** - Browser geolocation
3. **Route planning** - Directions to tasks
4. **Offline support** - Cached tiles

---

## Resources

- [Leaflet Docs](https://leafletjs.com/)
- [React-Leaflet Docs](https://react-leaflet.js.org/)
- [OpenStreetMap](https://www.openstreetmap.org/)
