# WebGL-Globe
Based on a fork of [globe.gl](https://github.com/vasturiano/globe.gl).

## Features

This fork adds enhanced support for country polygon visualization with:
- 🌍 Day/night cycle visualization with interactive UI controls
- 🗺️ API for loading country polygons with customizable colors
- 🔷 Support for multiple polygons per country (MultiPolygon)
- ⭕ Support for polygon holes (interior rings)
- ⏰ Interactive time-of-day controls with play/pause and slider
- 🎨 Customizable fill and stroke colors per polygon

## Quick Start

### Installation

```bash
npm install
npm run build
```

### Running the Example

1. Start a local HTTP server:
```bash
npm run dev
```

Or use Python:
```bash
python3 -m http.server 8080
```

2. Open your browser to:
```
http://localhost:8080/example/country-polygons-day-night/index.html
```

## Example

See the [Country Polygons with Day/Night Cycle](example/country-polygons-day-night/index.html) example for a full demonstration.

![Example Screenshot](https://github.com/user-attachments/assets/03b26377-5e26-472f-b3da-178619798cc3)

## API Documentation

For detailed API documentation, see [API.md](API.md).

### Quick API Overview

#### Load Country Polygons

```javascript
const polygons = [
  {
    name: 'Country Name',
    geometry: { type: "Polygon", coordinates: [[...]] },
    fillColor: '#ff6b6b',
    strokeColor: '#333333',
    altitude: 0.01
  }
];

window.GlobeAPI.loadCountryPolygons(polygons);
```

#### Set Time of Day

```javascript
window.GlobeAPI.setTime(14.5); // Set to 2:30 PM
```

#### Get Globe Instance

```javascript
const globe = window.GlobeAPI.getGlobe();
// Use any globe.gl API methods
```

## Polygon Types Supported

### Simple Polygon
```javascript
{
  type: "Polygon",
  coordinates: [[[outer ring coordinates]]]
}
```

### Polygon with Holes
```javascript
{
  type: "Polygon",
  coordinates: [
    [[outer ring coordinates]],
    [[hole 1 coordinates]],
    [[hole 2 coordinates]]
  ]
}
```

### MultiPolygon (Multiple Regions)
```javascript
{
  type: "MultiPolygon",
  coordinates: [
    [[[polygon 1 outer ring]], [[polygon 1 hole]]],
    [[[polygon 2 outer ring]]]
  ]
}
```

## Development

### Build the library
```bash
npm run build
```

### Development mode with auto-rebuild
```bash
npm run dev
```

### Project Structure
```
WebGL-Globe/
├── src/              # Source files from globe.gl
├── dist/             # Built library files
├── example/          # Example implementations
│   └── country-polygons-day-night/
│       └── index.html
├── API.md            # Detailed API documentation
└── README.md         # This file
```

## Features in Detail

### Time Control UI
- **Slider**: Drag to set time of day (0-23 hours)
- **Numeric Input**: Type exact time value
- **Play/Pause**: Start/stop automatic time progression
- **Reset to Now**: Jump to current system time
- **Auto-animate**: Toggle automatic time advancement

### Country Polygon API
The `loadCountryPolygons()` function accepts an array of polygon objects with:
- **geometry**: GeoJSON Polygon or MultiPolygon
- **fillColor**: Interior color (supports any CSS color format)
- **strokeColor**: Border/edge color
- **altitude**: Height above globe surface (0.01 = standard)
- **name**: Optional label for the polygon

### Multi-Polygon Support
Perfect for countries with:
- Non-contiguous territories (e.g., islands)
- Enclaves or exclaves
- Complex geographical boundaries

### Hole Support
Inner rings create holes in polygons, useful for:
- Lakes within countries
- Enclaves of other territories
- Complex coastal features

## Real-World Usage

Load actual country boundaries from GeoJSON:

```javascript
fetch('https://unpkg.com/world-atlas/countries-110m.json')
  .then(res => res.json())
  .then(data => {
    const countries = data.objects.countries.features.map(feature => ({
      name: feature.properties.name,
      geometry: feature.geometry,
      fillColor: `hsl(${Math.random() * 360}, 70%, 50%)`,
      strokeColor: '#333333',
      altitude: 0.01
    }));
    
    window.GlobeAPI.loadCountryPolygons(countries);
  });
```

## Browser Support

Requires a browser with WebGL support:
- Chrome 56+
- Firefox 52+
- Safari 11+
- Edge 79+

## Credits

Based on [globe.gl](https://github.com/vasturiano/globe.gl) by [Vasco Asturiano](https://github.com/vasturiano).

## License

MIT

