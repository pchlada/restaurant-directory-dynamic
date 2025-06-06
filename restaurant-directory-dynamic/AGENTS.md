# AGENTS.md - AI Agent Guide for Restaurant Directory Dynamic

## Project Overview for AI Agents

This is a **Single Page Application (SPA)** that dynamically generates a restaurant directory from JSON data. The entire application runs in the browser using vanilla JavaScript with no frameworks or build tools.

## Core Architecture Understanding

### Application Flow
1. **Initialization** → `js/app.js` loads and initializes all modules
2. **Data Loading** → `js/data.js` fetches restaurant JSON and processes geographic areas
3. **Routing** → `js/router.js` handles URL navigation and page rendering
4. **Template Rendering** → `js/templates.js` generates HTML for all page types

### Key Design Principles
- **Pure Vanilla JavaScript** - No frameworks, no build process
- **Client-Side Only** - Everything runs in the browser
- **Single Entry Point** - One HTML file serves all content
- **Hash-Based Routing** - Uses `#/path` for navigation
- **Dynamic Content Generation** - All HTML is generated from templates

## File Structure and Responsibilities

```
restaurant-directory-dynamic/
├── index.html              # Single entry point, loads all modules
├── css/style.css          # Complete styling with 6 color themes
├── js/
│   ├── app.js             # Application controller and initialization
│   ├── data.js            # Data management and geographic processing
│   ├── router.js          # Client-side routing system
│   └── templates.js       # HTML template generation engine
└── data/restaurants.json  # Restaurant data (100+ entries)
```

## Data Structure for AI Agents

### Restaurant Object Structure
When working with restaurant data, expect this JSON format:

```javascript
{
  "id": 42,                           // Auto-assigned unique ID
  "name": "Restaurant Name",          // Required: Display name
  "type": "Turkish restaurant",       // Required: Cuisine type
  "phone": "+44 20 1234 5678",       // Optional: Contact number
  "full_address": "123 St, London N4 1AB", // Required: Full address
  "postal_code": "N4 1AB",           // Required: For area grouping
  "rating": 4.5,                     // Required: 0-5 rating
  "reviews": 1250,                   // Required: Review count
  "photo": "https://image-url.jpg",   // Required: Primary image
  "working_hours": {                  // Optional: Operating hours
    "Monday": "9am-11pm",
    "Tuesday": "9am-11pm"
  },
  "about": {                         // Optional: Detailed amenities
    "Service options": {"Delivery": true},
    "Offerings": {"Halal food": true}
  },
  "location_link": "https://maps...", // Optional: Google Maps link
  "site": "https://website.com"      // Optional: Restaurant website
}
```

### Geographic Area Processing
Restaurants are automatically grouped into 6 areas based on postcodes:

```javascript
// Area grouping logic in data.js
const areaGroups = {
  'west-london': { postcodes: ['W11', 'W8', 'W10', 'W2'] },
  'east-london': { postcodes: ['IG1', 'IG3', 'E11', 'E18'] },
  'north-london': { postcodes: ['N4', 'N8', 'N15', 'N17', 'N22'] },
  'enfield-outer': { postcodes: ['EN4'] },
  'romford-essex': { postcodes: ['RM2', 'RM6'] },
  'other-areas': { postcodes: ['SE1', 'SW1', 'EC1', 'WC1'] }
};
```

## Module API Reference for AI Agents

### DataManager Class (`js/data.js`)
```javascript
// Global instance: dataManager
const dataManager = new DataManager();

// Key Methods:
await dataManager.loadData()                    // Load and process restaurant data
dataManager.getRestaurantById(id)              // Get restaurant by ID
dataManager.getAreaById(areaId)                 // Get area information
dataManager.searchRestaurants(query)           // Search across name/type/address
dataManager.getStats()                         // Get application statistics
dataManager.getColorTheme(index)               // Get theme for restaurant
```

### TemplateEngine Class (`js/templates.js`)
```javascript
// Global instance: templateEngine
const templateEngine = new TemplateEngine();

// Key Methods:
templateEngine.renderHomepage()                // Generate homepage HTML
templateEngine.renderAreaPage(areaId)          // Generate area listing page
templateEngine.renderRestaurantDetail(id)      // Generate restaurant detail page
templateEngine.renderRestaurantCard(restaurant, index) // Generate restaurant card
```

### Router Class (`js/router.js`)
```javascript
// Hash-based routing patterns:
// #/                     → Homepage
// #/area/west-london     → Area page
// #/restaurant/42        → Restaurant detail
// #/search/turkish       → Search results

// Navigation functions:
window.location.hash = '#/restaurant/42';      // Programmatic navigation
goBack();                                      // Go back in history
```

## AI Agent Development Guidelines

### When Adding New Features
1. **Data Changes** → Modify `js/data.js` and update JSON structure
2. **New Pages** → Add route pattern in `js/router.js` and template in `js/templates.js`
3. **UI Changes** → Update templates in `js/templates.js` and styles in `css/style.css`
4. **Functionality** → Add to appropriate module or create new module

### Common AI Agent Tasks

#### Adding a New Restaurant
```javascript
// 1. Add restaurant object to data/restaurants.json
// 2. Ensure required fields are present
// 3. Restaurant will automatically get ID and area assignment
```

#### Creating a New Page Type
```javascript
// 1. Add route pattern to router.routes in js/router.js
// 2. Create handler method in Router class
// 3. Add template method to TemplateEngine class
// 4. Update navigation if needed
```

#### Modifying Search Functionality
```javascript
// 1. Update searchRestaurants method in js/data.js
// 2. Modify search result template in js/templates.js
// 3. Update search UI in homepage template
```

### Error Handling Patterns
The application uses these error handling approaches:

```javascript
// Data loading errors
try {
  await dataManager.loadData();
} catch (error) {
  app.showError('Failed to load data');
}

// Image loading errors
<img src="${photo}" onerror="this.src='fallback-image.jpg'" />

// Routing errors
if (!routeFound) {
  this.show404();
}
```

### Color Theme System
Six themes rotate automatically based on restaurant index:

```javascript
const themes = ['purple', 'red', 'blue', 'green', 'orange', 'navy'];
const theme = themes[restaurantIndex % themes.length];
```

CSS classes follow pattern: `.theme-{color}`, `.card-{color}`, `.badge-{color}`

### Performance Considerations for AI Agents
- **Single Data Load** - All restaurant data loads once on app start
- **No Real-Time Updates** - Data is static until page refresh
- **Client-Side Only** - No server communication after initial load
- **Memory Efficient** - Templates generate HTML on demand

### Testing and Debugging
- **Browser Console** - All modules expose global instances for debugging
- **Network Tab** - Only one data request should occur (restaurants.json)
- **Hash Changes** - Watch URL hash changes for routing debugging
- **Local Development** - Serve files via local server for CORS compliance

### Integration Points for AI Agents
1. **Data Source** - Replace `data/restaurants.json` with your data
2. **Geographic Logic** - Modify area grouping in `processAreas()` method
3. **Template Customization** - Update HTML templates in `js/templates.js`
4. **Styling** - Modify CSS themes and responsive design
5. **Routing** - Add new URL patterns and handlers

### Common Modification Patterns

#### Adding a Filter Feature
```javascript
// 1. Add filter UI to homepage template
// 2. Create filter method in DataManager
// 3. Update search results rendering
// 4. Add route for filtered results
```

#### Integrating External APIs
```javascript
// 1. Add API calls to DataManager
// 2. Update restaurant object structure
// 3. Modify templates to display new data
// 4. Handle loading states appropriately
```

This guide should help AI agents understand the architecture and make informed modifications to the restaurant directory dynamic application. 