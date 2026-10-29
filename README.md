# Mini PWA Engine

**Live:** [https://sonusario.github.io/mini_pwa_engine/](https://sonusario.github.io/mini_pwa_engine/)

A visual builder for small progressive web apps. Lay out the screen, connect data, and say what buttons do; the engine writes the app's own code. There is no framework to install: the few helpers the app needs (about 60 lines to build and update the page) are written into the app itself, and only the parts the app uses are included.

The engine is one dependency-free page: `index.html`, plus `sw.js`, `manifest.json` and icons so it installs and works offline.

## Building an app

- **Add** components: Stack, Row, Grid and Card for layout; Heading, Text, Image, Link, Progress, Divider, Spacer, raw HTML and **Canvas** for content; Button, Text box, Long text, Checkbox, Dropdown and Slider for input; List, Table and Form for data.
- **Drag** components from Add onto the preview, or drag parts around inside the preview, to place them. A marker shows where a part will land: before, after, or inside a container. **Layers** shows the same page as a tree and takes drops too.
- **Resize** the selected part with the handles on its right, bottom and corner (sizes snap to 4px; Alt for 1px; double-click a handle to go back to automatic).
- **Layout** (in the Inspector) shows layout as pictures, like Godot's anchors and size flags:
  - *Position*: in the flow, or **pinned** to one of 16 anchor presets (corners, edges, center, wide and tall strips, full rect). Drag a pinned part in the preview to shift it from its anchor.
  - *Size*: Auto, Fill (take the free space) or Fixed, for width and height, plus a max width.
  - *Children* (containers): a 3×3 grid for where they line up, Packed or Spread out, Lined up or Stretched, and the gap.
  - *Margin and padding*: a box-model picture with a field for each side.
- **Inspector** edits the selected component: its text, what it is linked to, when it shows, its layout, its style, and its steps.
- **Text can show values** with `{{ expression }}`, for example `{{todos.filter(t => !t.done).length}} left`.
- **Steps** say what happens on a click, Enter, change or form submit: set state, flip a true/false value, add an item, change or delete the current item, delete all items, reload data, go to a page, show a message, continue only if a condition holds, or run your own JavaScript.
- **Canvas** runs your setup and per-frame code about 60 times a second, with keys, pointers and taps in `input`, and the app's state in `S`. The **Platformer** starter is a small game made this way.
- **Pages** each get an address (`#/path`) and optional tabs.
- **Data** holds *state* (values the app remembers) and *data sources* (lists of items with typed fields). A source is kept in memory, on the device (localStorage), at any JSON address (REST), or by a generated server (**Server API**), which keeps it in a JSON file, SQLite, PostgreSQL or **SQL Server**. The **Server database** form takes the server, port, instance, database, schema, user and sign-in (a SQL login, or your Windows account through a trusted connection); passwords only ever come from an environment variable.
- **App** sets the name, colors, light/dark, icon, offline support, the API address, and custom CSS and JavaScript.
- **Code** shows every file the export will contain, live. **Terminal** runs the text version of the app.
- **History** keeps versions of the project: commit with a message, see what changed since, compare any version with now, go back to one (Undo still works), and branch to try things. It lives in the browser, and **Save** puts it in the project file.
- **Console**, under the preview, shows the app's logs and errors. An error's stack trace names the component and step it came from (*Button "Add" › when clicked › step 2 (Run code)*), shows that line of the generated code, and has **Select it** to jump there. Errors from a server (with `DEBUG=1`) show its stack too.

Starters: To-do list, Counter, Shared notes (server API), Reader (two pages and a public REST API), **Platformer** (Canvas), **Mini Engine** (a page builder made with the engine), and Blank. Undo and redo cover every edit; projects are kept in the browser, and **Save** / **Open** use a `.json` file.

## What Export makes

Only what you tick. **Just the app** is four files; **Everything** adds the rest:

| File | |
| --- | --- |
| `index.html` | The app: HTML, CSS and plain JavaScript, with its icon built in. Always. |
| `manifest.json`, `icon.png`, `sw.js` | *Installable*: install and offline support. On by default. |
| `project.mpe.json` | The project, to open in the engine again. |
| `server/server.js` | Node server, one file: serves the app and, for Server API data, its REST API and database. No packages for JSON or SQLite. |
| `server/server.py` | The same server in Python, one file: standard library, plus `psycopg` or `pyodbc` for PostgreSQL or SQL Server. |
| `package.json` | Only when the Node code needs `pg`, `mssql` or `msnodesqlv8`. |
| `server/nginx.conf`, `server/Dockerfile`, `server/docker-compose.yml`, `server/schema.sql` | Optional: nginx, Docker (with a database container), and the tables as SQL. |
| `cli/app.js` | Optional: the terminal version of the same app. |
| `README.md` | Written for this app whenever there is more than the app itself to run. |

Both servers serve every file next to `index.html`, including sub-folders, keep `server/` and `cli/` private, and answer a missing file with a plain `404 Not found: <path>`. They make the database tables on the first run and add columns for fields added later. `API_ONLY=1` serves only the API (with CORS), and `DEBUG=1` sends stack traces back to the app.

### Talking to the database without the API

A browser can't open a database connection, so the web app always goes through the server. The terminal version can skip it: set **Data › Server database › Terminal version** to *Connects to the database directly*, and `cli/app.js` reads and writes the database itself, with the same settings. With SQL Server and *My Windows account*, the Node and Python code sign in as the Windows user running them (a trusted connection: `msnodesqlv8` for Node, `pyodbc` for Python, with Microsoft's ODBC driver).

### The same app without a screen

The terminal version shares the web app's state, data and action code; only the view differs. Each page prints as text and its buttons, inputs and links become a numbered menu, so the same design gives a provisional interface for places that don't usually have one, like a server shell or a device on a serial console.

## Goal: the engine building the engine

[TUTORIAL.md](TUTORIAL.md) builds **Mini Engine**, a small page builder with the engine's own shape (palette, parts list, preview, inspector, export), using only the engine's components and a code generator in custom JavaScript, the same way the engine is written. Its last section maps each of the engine's features to what builds it today; the editor's drag-and-drop and its generator are still custom JavaScript.

**About** (top right) links to this repository and the project hub, and lists the changes merged into `main`.
