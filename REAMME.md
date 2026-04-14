# From the Other Side | Wild Horizons

A small full-stack Node.js app for recording paranormal sightings and streaming live ghost news.

Built without Express, this project uses the native Node.js `http` module to handle routing, serve static files, read and write JSON data, parse request bodies, and stream live updates with Server-Sent Events.

---

## Features

- View all paranormal sightings
- Submit a new sighting
- Serve static frontend pages with raw Node.js
- Read and write sightings from a local JSON file
- Sanitize user input before saving
- Event-driven alerting with `EventEmitter`
- Live news feed using Server-Sent Events (SSE)
- Custom 404 page

---

## Tech Stack

### Backend
- Node.js
- Native `http` module
- Native `fs/promises`
- Native `path`
- Native `events`

### Frontend
- HTML
- CSS
- Vanilla JavaScript

### Data Storage
- Local JSON file (`data/data.json`)

### Dependency
- `sanitize-html`

---

## Project Structure

```text
from-the-other-side/
├── server.js
├── package.json
├── package-lock.json
├── hint.md
│
├── public/
│   ├── index.html
│   ├── sightings.html
│   ├── upload-sighting.html
│   ├── news.html
│   ├── index.js
│   ├── upload-sighting.js
│   ├── news.js
│   ├── index.css
│   ├── 404.html
│   └── images/
│       ├── candle-logo.png
│       └── ghostbg.jpg
│
├── handlers/
│   └── routeHandlers.js
│
├── utils/
│   ├── serveStatic.js
│   ├── sendResponse.js
│   ├── getContentType.js
│   ├── getData.js
│   ├── parseJSONBody.js
│   ├── addNewSighting.js
│   ├── sanitizeInput.js
│   └── createAlert.js
│
├── events/
│   └── sightingEvents.js
│
└── data/
    ├── data.json
    └── stories.js
```

## How It Works
### Static File Serving

The app serves frontend files directly from the public/ folder using a custom serveStatic() utility.

Examples:

- / → home page
- /sightings.html → sightings list
- /upload-sighting.html → form to submit sightings
- /news.html → live news feed
### API Routes
`GET /api`

Returns all saved sightings from `data/data.json`.

`POST /api`

Accepts a new sighting, parses the JSON body, sanitizes the input, stores it in the JSON file, and emits a custom event.

`GET /api/news`

Streams live paranormal news updates using Server-Sent Events (SSE).

## Architecture
### `server.js`

Acts as the entry point and routing layer.

It:

- creates the HTTP server
- checks req.url and req.method
- routes API requests to handlers
- serves static files for non-API routes
### `handlers/routeHandlers.js`

Contains route-level logic for:

- reading sightings
- creating sightings
- streaming live news
### `utils/`

Contains reusable helper functions:

- `serveStatic.js` → serves HTML, CSS, JS, and images
- `sendResponse.js` → standardizes responses
- `getContentType.js` → maps file extensions to MIME types
- `getData.js` → reads and parses sightings data
- `parseJSONBody.js` → manually parses incoming JSON request bodies
- `addNewSighting.js` → writes new sightings to the data file
- `sanitizeInput.js` → sanitizes user-submitted text
- `createAlert.js` → logs an alert when a new sighting is added
### `events/sightingEvents.js`

Uses Node’s EventEmitter to react to newly added sightings.

When a sighting is created, the app emits:
``` javascript
sightingEvents.emit('sighting-added', sanitizedBody)
```
That triggers the alert listener defined in `createAlert.js`.

## Frontend Pages
### `index.html`

Landing page for the app.

### `sightings.html`

Displays the stored sightings.

### `upload-sighting.html`

Contains the form for submitting a new sighting.

### `news.html`

Displays live incoming news updates using SSE.

### `404.html`

Custom page shown when a requested static route does not exist.

## Data Files
`data/data.json`

Stores all submitted sightings.

`data/stories.js`

Contains the live news stories used by the SSE route.
## Example Sighting Object
```JSON
{
  "title": "A ghostly encounter",
  "details": "I saw a strange figure near the old house.",
  "location": "Yorkshire"
}
```
## Learning Concepts Practiced

This project is a good introduction to backend fundamentals without relying on frameworks.

Key concepts include:

- Node.js HTTP server creation
- request/response handling
- manual routing
- static file serving
- file-based persistence
- JSON body parsing
- input sanitization
- event-driven programming with EventEmitter
- Server-Sent Events (SSE)
- separation of concerns

## Getting Started
### 1. Install dependencies
```bash
npm install
```
### 2. Start the server
```bash
npm start
```
### 3. Open in browser
```bash
http://localhost:8000
```

## Possible Improvements
- Add stronger validation for submitted sightings
- Improve error handling for API routes
- Clear SSE intervals when clients disconnect
- Refactor routing into a more scalable structure
- Replace JSON file storage with a database
- Add timestamps or IDs to sightings
- Add filtering or search for sightings
## Author

**M.Hammad Ullah**
* **LinkedIn:** [Click Here](www.linkedin.com/in/hammad-ullaah)

This project was built as a backend learning and portfolio piece to explore the core modules of **raw Node.js** without the abstraction of external frameworks.