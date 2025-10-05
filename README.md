# WebGL-Globe
Based on a fork of globe.gl.

## Features

This fork adds enhanced support for country polygon visualization with:
- Day/night cycle visualization with UI controls
- API for loading country polygons with customizable colors
- Support for multiple polygons per country
- Support for polygon holes (interior rings)
- Interactive time-of-day controls

## Example

See the [Country Polygons with Day/Night Cycle](example/country-polygons-day-night/index.html) example for a demonstration.

## API

### loadCountryPolygons(polygonData)

Loads country polygon data onto the globe with customizable colors.

**Parameters:**
- `polygonData` (Array): Array of objects, each representing a country or region with the following properties:
  - `geometry` (Object): GeoJSON geometry object (Polygon or MultiPolygon)
  - `fillColor` (String, optional): Color for the polygon fill (default: '#ffffaa')
  - `strokeColor` (String, optional): Color for the polygon edge (default: '#111111')
  - `altitude` (Number, optional): Altitude of the polygon (default: 0.01)
  - `name` (String, optional): Name of the country/region

**Returns:** The Globe instance for method chaining

**Example:**
```javascript
const polygons = [
  {
    geometry: { type: "Polygon", coordinates: [[...]] },
    fillColor: '#ff6b6b',
    strokeColor: '#333333',
    altitude: 0.01,
    name: 'Country Name'
  }
];

window.GlobeAPI.loadCountryPolygons(polygons);
```

### setTime(timeInHours)

Sets the time of day for the day/night cycle.

**Parameters:**
- `timeInHours` (Number): Time in 24-hour format (0-23, supports decimals)

**Example:**
```javascript
window.GlobeAPI.setTime(14.5); // Set to 2:30 PM
```

### getGlobe()

Returns the underlying Globe instance for advanced customization.

**Returns:** Globe instance

**Example:**
```javascript
const globe = window.GlobeAPI.getGlobe();
// Use any globe.gl API methods
```

## Multi-Polygon Support

The API supports both simple Polygons and MultiPolygons:

- **Polygon**: A single polygon with optional holes
- **MultiPolygon**: Multiple polygons (useful for countries with non-contiguous territories)

**Polygon with holes example:**
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

**MultiPolygon example:**
```javascript
{
  type: "MultiPolygon",
  coordinates: [
    [[[polygon 1 outer ring]], [[polygon 1 hole]]],
    [[[polygon 2 outer ring]]]
  ]
}
```

## Installation

```bash
npm install
npm run build
```

## Development

```bash
npm run dev
```

Then open `example/country-polygons-day-night/index.html` in your browser.

## License

MIT
