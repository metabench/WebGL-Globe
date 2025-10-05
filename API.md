# WebGL Globe API Documentation

## Overview

This WebGL Globe fork provides an enhanced API for loading and visualizing country polygons with customizable colors and time-of-day controls.

## Global API Access

The API is exposed through the `window.GlobeAPI` object in the example.

## Methods

### loadCountryPolygons(polygonData)

Loads country polygon data onto the globe with customizable colors and properties.

**Parameters:**
- `polygonData` (Array): An array of polygon objects, each with the following properties:

| Property | Type | Required | Default | Description |
|----------|------|----------|---------|-------------|
| `geometry` | Object | Yes | - | GeoJSON geometry object (Polygon or MultiPolygon) |
| `fillColor` | String | No | `'#ffffaa'` | Color for the polygon fill (any valid CSS color) |
| `strokeColor` | String | No | `'#111111'` | Color for the polygon edge/border |
| `altitude` | Number | No | `0.01` | Height of the polygon above the globe surface |
| `name` | String | No | `''` | Name/label for the country or region |

**Returns:** The Globe instance for method chaining

**Examples:**

#### Simple Polygon
```javascript
const polygons = [
  {
    name: 'Country A',
    geometry: {
      type: 'Polygon',
      coordinates: [[
        [-10, 30], [10, 30], [10, 50], [-10, 50], [-10, 30]
      ]]
    },
    fillColor: '#ff6b6b',
    strokeColor: '#333333',
    altitude: 0.015
  }
];

window.GlobeAPI.loadCountryPolygons(polygons);
```

#### Polygon with Hole
```javascript
const polygonWithHole = [
  {
    name: 'Country B',
    geometry: {
      type: 'Polygon',
      coordinates: [
        // Outer ring
        [[20, 30], [40, 30], [40, 50], [20, 50], [20, 30]],
        // Inner hole
        [[25, 35], [35, 35], [35, 45], [25, 45], [25, 35]]
      ]
    },
    fillColor: '#4ecdc4',
    strokeColor: '#000000',
    altitude: 0.02
  }
];

window.GlobeAPI.loadCountryPolygons(polygonWithHole);
```

#### MultiPolygon (Multiple Separate Regions)
```javascript
const multiPolygon = [
  {
    name: 'Archipelago Country',
    geometry: {
      type: 'MultiPolygon',
      coordinates: [
        // First island
        [[[50, 30], [60, 30], [60, 40], [50, 40], [50, 30]]],
        // Second island
        [[[65, 35], [75, 35], [75, 45], [65, 45], [65, 35]]]
      ]
    },
    fillColor: '#95e1d3',
    strokeColor: '#333333',
    altitude: 0.015
  }
];

window.GlobeAPI.loadCountryPolygons(multiPolygon);
```

### setTime(timeInHours)

Sets the time of day for visualization and the day/night cycle.

**Parameters:**
- `timeInHours` (Number): Time in 24-hour format (0-23). Supports decimal values for minutes (e.g., 14.5 = 2:30 PM)

**Returns:** void

**Examples:**

```javascript
// Set to noon
window.GlobeAPI.setTime(12);

// Set to 2:30 PM
window.GlobeAPI.setTime(14.5);

// Set to midnight
window.GlobeAPI.setTime(0);

// Set to 6:45 AM
window.GlobeAPI.setTime(6.75);
```

### getGlobe()

Returns the underlying Globe instance for advanced customization using the full globe.gl API.

**Parameters:** None

**Returns:** Globe instance

**Examples:**

```javascript
const globe = window.GlobeAPI.getGlobe();

// Use any globe.gl method
globe
  .pointOfView({ lat: 40, lng: -100, altitude: 2 })
  .showAtmosphere(true)
  .atmosphereColor('lightskyblue');
```

## GeoJSON Geometry Support

The API supports standard GeoJSON geometry types:

### Polygon

A polygon is defined by one or more linear rings. The first ring is the outer boundary, and any subsequent rings are holes.

```javascript
{
  type: 'Polygon',
  coordinates: [
    [[outer_ring_coordinates]],
    [[hole_1_coordinates]],  // optional
    [[hole_2_coordinates]]   // optional
  ]
}
```

### MultiPolygon

A MultiPolygon consists of multiple polygons (useful for countries with non-contiguous territories).

```javascript
{
  type: 'MultiPolygon',
  coordinates: [
    [[[polygon_1_outer]], [[polygon_1_hole]]],  // optional holes
    [[[polygon_2_outer]]]
  ]
}
```

## Color Formats

Colors can be specified in any valid CSS color format:

- **Hex**: `'#ff6b6b'`, `'#f00'`
- **RGB**: `'rgb(255, 107, 107)'`
- **RGBA**: `'rgba(255, 107, 107, 0.8)'`
- **HSL**: `'hsl(0, 100%, 71%)'`
- **Named colors**: `'red'`, `'blue'`, `'lightblue'`

## Complete Usage Example

```javascript
// Define polygon data
const countries = [
  {
    name: 'Sample Country 1',
    geometry: {
      type: 'Polygon',
      coordinates: [[
        [-10, 30], [10, 30], [10, 50], [-10, 50], [-10, 30]
      ]]
    },
    fillColor: '#ff6b6b',
    strokeColor: '#333333',
    altitude: 0.015
  },
  {
    name: 'Sample Country 2',
    geometry: {
      type: 'MultiPolygon',
      coordinates: [
        [[[50, 30], [60, 30], [60, 40], [50, 40], [50, 30]]],
        [[[65, 35], [75, 35], [75, 45], [65, 45], [65, 35]]]
      ]
    },
    fillColor: '#4ecdc4',
    strokeColor: '#000000',
    altitude: 0.02
  }
];

// Load the polygons
window.GlobeAPI.loadCountryPolygons(countries);

// Set time to sunrise (6 AM)
window.GlobeAPI.setTime(6);

// Get the globe for additional customization
const globe = window.GlobeAPI.getGlobe();
globe.pointOfView({ lat: 40, lng: -100, altitude: 2 });
```

## Real-world Data Integration

To load real country boundaries, you can fetch GeoJSON data:

```javascript
fetch('https://unpkg.com/world-atlas/countries-110m.json')
  .then(res => res.json())
  .then(data => {
    const countries = data.objects.countries.features.map(feature => ({
      name: feature.properties.name,
      geometry: feature.geometry,
      fillColor: getColorForCountry(feature.properties.name),
      strokeColor: '#333333',
      altitude: 0.01
    }));
    
    window.GlobeAPI.loadCountryPolygons(countries);
  });
```

## Notes

- Coordinates are in longitude/latitude format: `[longitude, latitude]`
- Longitude ranges from -180 to 180 (west to east)
- Latitude ranges from -90 to 90 (south to north)
- Polygons must have closed rings (first and last coordinate must be the same)
- For best performance, simplify complex geometries when possible
- The globe uses WebGL for rendering, so ensure browser supports WebGL
