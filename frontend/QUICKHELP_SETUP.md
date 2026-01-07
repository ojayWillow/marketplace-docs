# Quick Help Feature Setup

## Overview

The Quick Help feature is an interactive map-based task marketplace where users can find and complete local jobs.

---

## Features

### 🗺️ Interactive Map
- Leaflet.js with React-Leaflet
- OpenStreetMap tiles (free)
- Centered on Riga, Latvia
- Zoom level 13 (neighborhood view)

### 📍 Custom Markers
- Price labels on markers (€25, €45, etc.)
- Color-coded by budget tier
- Click for task details
- Accept task from popup

### 📋 Task List
- All tasks below map
- Category badges
- Distance indicators
- Accept buttons

### 🔄 Task Workflow
```
open → assigned → pending_confirmation → completed
```

1. **Creator** posts task
2. **Worker** applies
3. **Creator** accepts application
4. **Worker** marks as done
5. **Creator** confirms completion

---

## Three-Tab Interface

| Tab | Shows |
|-----|-------|
| All | Tasks + Offerings combined |
| Jobs | Only tasks (requests for help) |
| Offerings | Only services (people offering help) |

---

## Location Features

### Address Search
- Nominatim geocoding (free)
- Latvia-focused results
- Autocomplete suggestions

### Manual Selection
- Click anywhere on map
- Marker shows selected location
- Coordinates saved automatically

### Saved Location
- Stored in localStorage
- Persists between sessions
- Can be updated anytime

---

## Dependencies

```json
{
  "leaflet": "^1.9.4",
  "react-leaflet": "^4.2.1",
  "@types/leaflet": "^1.9.8"
}
```

---

## Key Components

- `Tasks.tsx` - Main page with map and list
- `TaskCard.tsx` - Individual task display
- `CreateTask.tsx` - Task creation form
- `LocationPicker.tsx` - Map-based location selector

---

## API Integration

### Get Tasks
```
GET /api/tasks?latitude=56.9496&longitude=24.1052&radius=10
```

### Create Task
```
POST /api/tasks
{
  "title": "Dog walking",
  "budget": 25,
  "latitude": 56.9496,
  "longitude": 24.1052
}
```

### Apply for Task
```
POST /api/tasks/:id/apply
{
  "message": "I can help!"
}
```

---

## Matching Feature

If user has offerings that match nearby tasks:
- Notification badge appears
- "Jobs matching your skills" section
- One-click apply

---

## Related Documentation

- [Map Integration](MAP_INTEGRATION.md)
- [Backend Tasks API](../backend/API_ENDPOINTS.md#tasks-quick-help)
