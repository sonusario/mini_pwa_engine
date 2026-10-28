# Mini PWA Engine

**Live:** [https://sonusario.github.io/mini_pwa_engine/](https://sonusario.github.io/mini_pwa_engine/)

A visual builder for small progressive web apps. Lay out the screen, connect data, and say what buttons do; the engine writes the app's own code. There is no framework to install: the few helpers the app needs (about 60 lines to build and update the page) are written into the app itself, and only the parts the app uses are included.

The engine is one dependency-free page: `index.html`, plus `sw.js`, `manifest.json` and icons so it installs and works offline.

## Building an app

- **Add** components: Stack, Row and Card for layout; Heading, Text, Image, Link, Progress, Divider, Spacer and raw HTML for content; Button, Text box, Long text, Checkbox, Dropdown and Slider for input; List, Table and Form for data.
- **Layers** shows the page as a tree. Drag to reorder, or drop onto the middle of a container to put something inside.
- **Preview** is the real generated app. In *Select* mode a click picks a component; in *Use* mode it runs. Phone, tablet and wide sizes. A console under it shows the app's logs and errors.
- **Inspector** edits the selected component: its text, what it is linked to, when it shows, its style, and its steps.
- **Text can show values** with `{{ expression }}`, for example `{{todos.filter(t => !t.done).length}} left`.
- **Steps** say what happens on a click, Enter, change or form submit: set state, flip a true/false value, add an item, change or delete the current item, delete all items, reload data, go to a page, show a message, continue only if a condition holds, or run your own JavaScript.
- **Pages** each get an address (`#/path`) and optional tabs.
- **Data** holds *state* (values the app remembers) and *data sources* (lists of items with typed fields). A source is kept in memory, on the device (localStorage), at any JSON address (REST), or by a generated server (**Server API**).
- **App** sets the name, colors, light/dark, icon, offline support, the API address, and custom CSS and JavaScript.
- **Code** shows every file the export will contain, live. **Terminal** runs the text version of the app.

Starters: To-do list, Counter, Shared notes (server API), Reader (two pages and a public REST API), and Blank. Undo and redo cover every edit; projects are kept in the browser, and **Save** / **Open** use a `.json` file.

## What Export makes

A `.zip` that works as-is on GitHub Pages or any static host:

| File | |
| --- | --- |
| `index.html` | The app: HTML, CSS and plain JavaScript. |
| `sw.js`, `manifest.json`, `icon.svg`, `icon-192.png`, `icon-512.png` | Offline support and install. |
| `project.mpe.json` | The project, to open in the engine again. |
| `README.md` | Setup notes for the files below, written for this app. |
| `server/web.js` | Web server (Node, no packages). |
| `server/web.py`, `server/nginx.conf` | Optional: Python web server, nginx config. |
| `server/api.js` | API server: REST for each Server API source, with CORS. |
| `server/app.js` | Application server: the app and its API on one port, with server-side rules in `server/lib/hooks.js`. |
| `server/lib/store.js`, `server/db/*.sql` | Database: a JSON file, SQLite (Node's built-in `node:sqlite`), or PostgreSQL (with `docker-compose.yml`). |
| `server/Dockerfile`, `server/docker-compose.yml` | Optional: Docker. |
| `cli/app.js` | Terminal version of the same app. |

The API, application server and database are only generated when the app has a Server API data source; they follow its fields.

### The same app without a screen

The terminal version shares the web app's state, data and action code; only the view differs. Each page prints as text and its buttons, inputs and links become a numbered menu, so the same design gives a provisional interface for places that don't usually have one, like a server shell or a device on a serial console.

## Goal: the engine building the engine

The aim is for apps this small to cover anything, up to the engine itself. It isn't there yet: the engine's own editor (tree drag-and-drop, the inspector, the code generator) is beyond what its components can express without the HTML component and custom JavaScript.

**About** (top right) links to this repository and the project hub, and lists the changes merged into `main`.
