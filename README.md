# Regex Log Parser Website (SvelteKit)

A minimal, client-side developer utility and classroom lab where students learn how **raw log → regex → named groups → JSON**.

---

## Features

- **Client-Side Only**: Runs completely in the browser with standard `RegExp`, File API, and `Blob` JSON export.
- **Log Source Selector**: Switch between predefined datasets (`Apache Access`, `SSH Authentication`, `Application Events`) or upload custom `.log`/`.txt` files without server round-trips.
- **Interactive Named Groups Regex**: Explains and demonstrates `(?<field>pattern)` mapping directly to JSON object keys.
- **Instant Output & Match Diagnostics**: Live match statistics (lines total, matched, failed) and a dedicated **UNMATCHED** section to inspect rejected entries.
- **Download JSON**: Generates and downloads formatted JSON results on demand.
- **Example Presets & Classroom Challenges**: Quick-load patterns and 5 structured student milestones.

---

## Project Structure

```text
log-parser/
├── src/
│   ├── routes/
│   │   ├── +layout.js      # Prerender / client-side SPA mode
│   │   ├── +layout.svelte  # Global layout importing app.css
│   │   └── +page.svelte    # Complete workbench UI and reactive parser logic
│   ├── app.css             # Developer-lab dark theme styling
│   └── app.html
├── static/
│   └── logs/
│       ├── apache.log
│       ├── ssh.log
│       └── application.log
├── vite.config.js          # SvelteKit with @sveltejs/adapter-static
└── package.json
```

---

## Running Locally

```bash
cd log-parser
npm install
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

---

## Building for Production / GitHub Pages

```bash
npm run build
```

The static bundle is generated in the `build/` directory ready for deployment to GitHub Pages or any static host.
