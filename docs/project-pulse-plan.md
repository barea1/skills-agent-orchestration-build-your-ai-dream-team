# Project Pulse implementation plan

## Summary

Build Mona's Project Pulse as a dependency-free static dashboard for contributors. It will present multiple projects in accessible, responsive cards showing each project's name, owner, status, recent activity, and priority. The app will run from `app/` with Python's built-in HTTP server and will be opened at `index.html`, rather than at a directory listing.

The repository brief is `.github/project-pulse-brief.md`. The custom agent definitions are in `.github/agents/`. The required learner files do not yet exist, so implementation will create them without changing the existing agent definitions, `.vscode/tasks.json`, or dev-container configuration.

## Ordered implementation steps

1. **Confirm requirements and ownership (Planner and Orchestrator).** Review the project brief and repository configuration, confirm the required project fields and runnable-app behavior, and assign non-overlapping file ownership.
2. **Define the experience (Designer).** Establish the information hierarchy, accessible status and priority treatments, responsive layout, typography, spacing, and visual hooks. The design must include `.dashboard` and `.project-card`.
3. **Create representative data (Coder).** Create a valid `projects` array with multiple project objects. Every object must include `name`, `owner`, `status`, `recentActivity`, and `priority`.
4. **Implement the document and rendering (Coder, guided by Designer).** Create the semantic page structure, load `project-data.json`, render all required fields into visible project cards, and provide explicit loading, empty, and error states.
5. **Implement styling (Designer with Coder integration).** Create the responsive dashboard grid, polished cards with rounded corners and shadows, readable hierarchy, accessible contrast, and visible focus states.
6. **Create the launch configuration (Coder).** Create strict JSON for `Run Project Pulse Dashboard`, using `python3 -m http.server 5500`, `cwd` of `${workspaceFolder}/app`, and a server-ready URL of `http://localhost:%s/index.html`.
7. **Integrate and review (Orchestrator).** Check that JSON fields, HTML rendering, CSS selectors, relative asset paths, and launch settings agree before runtime validation.

## File assignments

| File | Owner | Responsibility |
|---|---|---|
| `app/index.html` | Coder, guided by Designer | Semantic dashboard structure, data loading, project-card rendering, accessibility, and visible required fields |
| `app/styles.css` | Designer, with Coder integration | Responsive layout, visual hierarchy, status and priority presentation, focus states, `.dashboard`, and `.project-card` styling |
| `app/project-data.json` | Coder | Valid top-level `projects` array with multiple consistent project records and required fields |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration serving `app/` on port 5500 and opening `index.html` |
| `docs/project-pulse-plan.md` | Planner, coordinated by Orchestrator | Implementation plan, ownership, dependencies, sequencing, risks, and validation expectations |

## Designer responsibilities

The Designer owns the user experience and visual direction. They will define a contributor-friendly information hierarchy; semantic and accessible patterns; readable typography and spacing; status and priority treatments that do not rely on color alone; responsive behavior for mobile, tablet, and desktop; and the polished card treatment using rounded corners, shadows, contrast, and clear focus states. The Designer will ensure `.dashboard` and `.project-card` are stable CSS hooks and will review the integrated HTML/CSS result. The Designer will not change the launch configuration or data contract unless the Orchestrator explicitly reassigns that work.

## Coder responsibilities

The Coder owns implementation within the assigned files. They will create and connect the HTML, CSS, and JSON; render deterministic cards from the `projects` data; preserve the required field names; handle malformed, empty, or missing data explicitly; and create the strict `.vscode/launch.json` configuration. The solution will remain dependency-free and compatible with the Codespace. The Coder will keep the page runnable over HTTP and will not stage, commit, or push changes.

## Dependencies

- The project brief must be reviewed before implementation and is the source of required content and launch behavior.
- Designer decisions about hierarchy, accessibility, responsive behavior, and CSS hooks must be available before the Coder finalizes the page.
- The JSON schema must be established before rendering is integrated; HTML must use the exact data field names.
- HTML and CSS must agree on `.dashboard`, `.project-card`, and any status or priority classes.
- The app files must exist before the launch configuration can be tested end to end.
- Browser validation must use the completed HTML, CSS, JSON, and launch configuration together. `fetch` should be tested over HTTP, not by opening the HTML with a `file://` URL.

## Parallel work decisions

After requirements and ownership are confirmed, the Designer can develop the information hierarchy and visual direction in parallel with the Coder creating `app/project-data.json`. The Coder can also draft `.vscode/launch.json` independently because its command, port, working directory, and target URL are fixed.

Work must be sequential when it has dependencies: the Planner and Orchestrator finalize ownership first; Designer and Coder reconcile design guidance with the data shape and markup hooks; the Coder integrates the final HTML, CSS, and JSON; the launch configuration is checked against the completed app; and the Orchestrator performs the integrated review before browser validation. Parallel edits must not overlap the same file, especially `app/index.html` and `app/styles.css`.

## Edge cases and risks

- Malformed or missing JSON must produce a visible error state rather than a silent empty dashboard.
- An empty `projects` array must produce a clear empty-state message.
- Missing fields should use an explicit fallback such as `Not provided`.
- Long project names and activity text must wrap without horizontal overflow.
- Unknown status or priority values should render as text with neutral styling.
- Status and priority must remain understandable without color.
- The launch URL must end in `/index.html` to avoid showing a directory listing.
- `app/project-data.json` and `.vscode/launch.json` must be strict JSON with no comments or trailing commas.
- Port conflicts should be reported during local troubleshooting; the committed configuration should retain the required port.

## Validation expectations

### Static validation

- Confirm that `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json` exist.
- Parse both JSON files with an available JSON parser.
- Confirm the data has a top-level `projects` array and that every project has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Confirm `index.html` has the exact `Project Pulse` title, references `styles.css` and `project-data.json`, uses `.dashboard` and `.project-card`, and renders status, recent activity, and priority.
- Confirm `styles.css` contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- Confirm `launch.json` contains `Run Project Pulse Dashboard`, `python3 -m http.server 5500`, the `app` working directory, and an `index.html` target, with no comments.

### Runtime and accessibility validation

Start `Run Project Pulse Dashboard` from VS Code. Confirm it serves from `app/`, opens `http://localhost:%s/index.html` with the resolved port, loads the JSON without console errors, and displays multiple project cards with all required fields. Resize the viewport to check responsive behavior, then verify keyboard focus visibility, readable contrast, content wrapping, loading/error/empty states, and that status and priority remain clear without color. The repository's workflow checks supplement this browser review.

## Assumptions

No backend, persistence, authentication, filtering, sorting, framework, package manager, or build pipeline is required. The dashboard is a small static app intended to run in the existing Codespace. GitHub Copilot CLI in the Codespace, coordinated by the Orchestrator, will manage the Planner, Designer, and Coder workflow.
