# Project Pulse implementation plan

## Goal

Build a lightweight, static Project Pulse dashboard that lets Mona's contributors
quickly see which projects are active, who owns them, their status, recent
activity, and priority or risk. Present the information as a polished,
responsive, accessible collection of project cards. Include a short,
contributor-friendly summary for each project so people can understand its
purpose or current focus at a glance.

## Ordered implementation phases

### 1. Agree on the data contract and design

- **Designer:** Define the information hierarchy and visual direction for the
  dashboard, including card layout, status badges, priority/risk treatment,
  readable spacing, responsive behavior, color contrast, and keyboard and
  screen-reader considerations. Ensure the summary is easy to find without
  obscuring the required project fields.
- **Coder:** Confirm the data shape and static-app constraints before
  implementation; do not introduce a framework or build step.
- **Output:** A shared contract: `project-data.json` has a top-level `projects`
  array, with each project containing `name`, `owner`, `status`,
  `recentActivity`, and `priority`, plus a concise `summary` for contributors.
- **Dependency:** Complete this phase before implementation so markup, styles,
  and data use the same structure.

### 2. Create representative project data

- **File assignment — `app/project-data.json` (Coder):** Add a valid JSON
  document with a top-level `projects` array and multiple realistic sample
  projects. Every project must provide non-empty `name`, `owner`, `status`,
  `recentActivity`, `priority`, and contributor-friendly `summary` values.
  Keep status and priority values concise and consistent so they can be
  displayed as badges or labels.
- **Dependency:** The data contract from Phase 1. The HTML renderer depends on
  this schema and these field names.

### 3. Build the dashboard structure and presentation

- **File assignment — `app/index.html` (Coder):** Create the page with the exact
  title “Project Pulse”, link `styles.css`, and load `project-data.json`.
  Render visible cards from the `projects` array using the `project-card` class.
  Show each project's name, owner, status, recent activity, priority, and
  summary. Include meaningful headings, semantic structure, and accessible
  text for status/priority rather than relying on color alone. Handle data-load
  errors and an empty project list with understandable messages.
- **File assignment — `app/styles.css` (Coder, guided by Designer):** Implement
  the `.dashboard` and `.project-card` selectors, clear visual hierarchy,
  status badges, priority treatment, readable spacing and typography, rounded
  corners, subtle shadows, and a responsive layout. Include visible
  focus states and sufficient contrast.
- **Dependency:** The HTML and CSS are coupled through their class names and
  the design decisions from Phase 1. The page's data rendering depends on the
  schema and sample data from Phase 2.

### 4. Configure the preview launch

- **File assignment — `.vscode/launch.json` (Coder):** Add strict JSON with a
  configuration named `Run Project Pulse Dashboard`. Run
  `python3 -m http.server 5500` with `cwd` set to
  `${workspaceFolder}/app`. Use `serverReadyAction` to open
  `http://localhost:%s/index.html`, so the preview opens the dashboard rather
  than the server's directory listing.
- **Dependency:** The static app must exist at `app/index.html`; the launch
  configuration serves that directory and explicitly opens the page.

### 5. Integrate and validate

- **Coder:** Check cross-file field names, references, selectors, and launch
  settings; make corrections within the assigned files.
- **Designer:** Review the rendered result for hierarchy, responsive behavior,
  readability, status/priority clarity, and accessibility; report any design
  adjustments needed to Coder.
- **Dependency:** Phases 2–4 must be integrated before the end-to-end preview
  review.

## Roles and file ownership

- **Orchestrator:** Coordinate the work, use this plan as the handoff contract,
  delegate visual/accessibility decisions to Designer and implementation to
  Coder, and ensure the integration checks are completed.
- **Planner:** Establish phases, dependencies, file ownership, risks, and
  validation expectations as documented here; does not implement application
  files.
- **Designer:** Own design guidance and review, including layout, information
  hierarchy, accessible presentation, and responsive visual behavior. The
  Designer does not edit files unless explicitly assigned.
- **Coder:** Own implementation of `app/index.html`, `app/styles.css`,
  `app/project-data.json`, and `.vscode/launch.json`, following Designer's
  decisions and this data contract.

## Dependencies and parallel work decisions

- Phase 1 is a prerequisite for implementation because it settles the data
  fields and visual hierarchy.
- After Phase 1, data authoring (Phase 2) and independent design exploration
  by Designer can proceed in parallel. Coder should wait for the agreed
  contract before finalizing the renderer.
- The HTML renderer and JSON data are logically dependent on the shared field
  contract; they may be drafted in parallel only if both follow that contract,
  then must be integrated and tested together.
- CSS and HTML can be developed in parallel after Designer establishes the
  layout and component/class contract. Keep Coder as the owner of both files
  to avoid conflicting edits; Designer supplies guidance and reviews.
- `launch.json` can be authored in parallel with app implementation because
  its command and path are known, but its preview validation must wait until
  `app/index.html` is present.
- Phase 5 is sequential after all assigned files are integrated.

## Edge cases and assumptions

- The app is static and served over HTTP; loading the JSON with `fetch` from a
  page opened directly as a `file://` URL may fail. Validate through the
  provided local-server launch configuration.
- Handle an empty or missing `projects` array, failed JSON fetch, and malformed
  project entries gracefully; show a useful message rather than a blank page
  or uncaught failure.
- Treat project text as data, not trusted HTML; render it as text to avoid
  interpreting sample values as markup.
- Status and priority vocabularies are not prescribed. Use a small, consistent
  set of representative values and ensure labels remain understandable without
  color.
- The brief requires the five named project fields; `summary` is an additional
  field required to satisfy its contributor-friendly summary goal.
- No framework, external dependency, backend, or project-specific sample
  content is specified; keep implementation dependency-free and use clearly
  illustrative sample data.

## Validation expectations

- Parse `app/project-data.json` as JSON; confirm it has a top-level `projects`
  array and that every project includes non-empty `name`, `owner`, `status`,
  `recentActivity`, `priority`, and `summary`.
- Inspect `app/index.html` to confirm the exact “Project Pulse” title,
  stylesheet and JSON references, semantic/accessibility support, and visible
  `.project-card` rendering of every required field including summary.
- Inspect `app/styles.css` for `.dashboard`, `.project-card`, responsive
  behavior, readable contrast and focus treatment, `border-radius`, and
  `box-shadow`.
- Parse `.vscode/launch.json` as strict JSON. Confirm the named launch
  configuration, server command, `app/` working directory, and
  `http://localhost:%s/index.html` browser target.
- Run **Run Project Pulse Dashboard** and verify that the browser displays the
  dashboard (not a directory listing); check the empty/error states and a
  narrow viewport as well as the normal populated view.
