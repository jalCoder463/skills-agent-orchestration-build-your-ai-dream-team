# Project Pulse implementation plan

## Summary

Build a runnable Project Pulse dashboard as a small static web application. The
dashboard will be served from `app/` with Python's built-in HTTP server and
will read project records from a JSON data file. The implementation should
provide a clear, accessible, responsive project overview while keeping visual
decisions, application implementation, and orchestration responsibilities
separate.

The required outputs are:

- `app/index.html` — dashboard structure and entry point.
- `app/styles.css` — visual system, layout, responsive behavior, and
  accessibility-related presentation.
- `app/project-data.json` — fixture data with a top-level `projects` JSON
  array.
- `.vscode/launch.json` — a deterministic launch configuration that serves
  `app/` on port `5500` and opens
  `http://localhost:%s/index.html`, not a directory listing.

`app/` and the required `.vscode/launch.json` do not yet exist in the baseline,
so the implementation must create them.

## Ordered implementation steps

1. **Orchestrator establishes the contract.** Confirm the four-file scope,
   ownership boundaries, data shape, server command, port, and launch URL.
   Resolve any ambiguity before implementation begins.
2. **Designer defines the dashboard direction.** Specify the page hierarchy,
   project-card or project-row structure, status and progress treatments,
   responsive layout, typography, color usage, focus states, and accessible
   interaction states. The design should be concrete enough for the Coder to
   implement directly in the assigned files.
3. **Coder creates the data fixture.** Add `app/project-data.json` with a
   top-level `projects` array and representative records covering normal,
   complete, at-risk, empty, and long-content cases as appropriate to the
   dashboard design.
4. **Coder implements the page.** Add `app/index.html`, load the stylesheet
   and project data using paths relative to `app/`, and render the agreed
   dashboard structure. Keep the page usable if data loading fails by exposing
   an explicit, user-visible error state rather than silently showing an
   empty dashboard.
5. **Coder implements presentation.** Add `app/styles.css` according to the
   Designer's direction, including responsive behavior, readable contrast,
   visible keyboard focus, and layouts that remain usable at narrow and wide
   viewport sizes.
6. **Orchestrator adds the runnable workflow.** Create `.vscode/launch.json`
   with a configuration that runs `python3 -m http.server 5500` from
   `${workspaceFolder}/app` and opens
   `http://localhost:%s/index.html`. The URL must target the document directly
   so users do not land on a directory listing.
7. **Orchestrator integrates and validates.** Review all four files together,
   verify that the JSON contract matches the rendering logic, check launch
   configuration paths and placeholders, and run the validation checks below.
   Any integration issue is assigned back to the owner of the affected file.

## File assignments

| File | Owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder | Implement the semantic dashboard shell, data loading/rendering behavior, headings, project content regions, loading state, and explicit error state. Apply the Designer's structure without moving visual rules into inline styles. |
| `app/styles.css` | Coder, guided by Designer | Implement the visual design, responsive layout, status/progress styling, spacing, typography, contrast, reduced-motion considerations, and keyboard focus presentation. |
| `app/project-data.json` | Coder, based on Designer's content needs | Provide valid fixture data in a top-level `projects` array, with fields and values sufficient to exercise each rendered state. |
| `.vscode/launch.json` | Orchestrator | Provide the launch configuration for `python3 -m http.server 5500` with working directory `${workspaceFolder}/app` and direct URL `http://localhost:%s/index.html`. Keep the file valid strict JSON. |

## Designer responsibilities

- Define the information hierarchy and the visual language for Project Pulse.
- Specify how project name, owner or team, status, progress, dates, and other
  project metadata are prioritized.
- Define status distinctions that do not rely on color alone, including text
  labels and suitable semantic or visual cues.
- Define responsive behavior for small, medium, and wide viewports without
  requiring horizontal scrolling for core content.
- Define accessible typography, contrast, spacing, focus states, and motion
  behavior.
- Provide implementation-ready guidance to the Coder and review the integrated
  result for visual and accessibility consistency.

## Coder responsibilities

- Implement only the assigned application files and follow the agreed data
  contract.
- Keep HTML semantic and keyboard-friendly, with a logical heading structure,
  meaningful labels, and appropriate alternative text or decorative treatment.
- Load and render the top-level `projects` array deterministically.
- Handle loading, malformed data, empty data, and fetch failures with clear
  user-facing states.
- Keep styling in `app/styles.css` and ensure the page works when served from
  the `app/` directory.
- Validate the implementation in a browser or equivalent local HTTP request and
  report any dependency or integration issue to the Orchestrator.

## Dependencies

- The Designer's structure and field requirements are needed before the Coder
  finalizes the fixture shape and markup.
- `app/project-data.json` and `app/index.html` must agree on property names and
  expected value types.
- `app/index.html` depends on `app/styles.css` being available at the relative
  stylesheet path.
- Browser `fetch` behavior requires an HTTP server; opening `index.html`
  directly from the filesystem is not a valid substitute for testing.
- `.vscode/launch.json` depends on the final `app/` location and must use the
  same port and server command used in validation.

## Parallel versus sequential work

The Orchestrator's contract-setting and the Designer's initial visual
specification are sequential prerequisites for implementation. After the
Designer defines the data fields and page structure, the Coder can work on
`app/project-data.json` and the initial `app/index.html` in parallel with the
Coder's stylesheet work, provided the interface contract is fixed first.

The launch configuration can be drafted in parallel with application
implementation because its command and paths are predetermined, but final
validation is sequential: the configuration must be tested against the actual
created `app/` directory, and the integrated page must be checked with the
final data and styles. Orchestrator integration and end-to-end validation occur
after all four required files exist.

## Edge cases and risks

- **Invalid or missing JSON:** show a clear error state and avoid an
  uninformative blank page.
- **An empty `projects` array:** show an intentional empty-state message
  rather than a broken grid or misleading totals.
- **Missing optional fields:** use an explicit fallback or omit the field
  cleanly; do not render `undefined`, `null`, or malformed labels.
- **Long names and metadata:** allow wrapping or truncation that preserves
  access to the complete value and does not break the layout.
- **Unexpected status values:** provide a readable fallback treatment and do
  not depend solely on a fixed color mapping.
- **Narrow viewports and zoom:** prevent clipped controls and horizontal
  overflow; preserve readable text and usable touch targets.
- **Accessibility:** maintain heading order, keyboard focus visibility,
  sufficient contrast, meaningful status text, and non-color indicators.
- **Server/URL mismatch:** a wrong working directory or URL can expose a
  directory listing or make data requests fail; verify both the launch command
  and the direct `index.html` URL.
- **Relative paths:** test from the configured `app/` server root so stylesheet
  and JSON requests behave the same way they will in the launch configuration.

## Validation expectations

- Confirm exactly these required outputs exist:
  `app/index.html`, `app/styles.css`, `app/project-data.json`, and
  `.vscode/launch.json`.
- Parse `app/project-data.json` as JSON and verify that `projects` exists and
  is an array.
- Validate that `.vscode/launch.json` is strict JSON and that its launch
  configuration uses `python3 -m http.server 5500`, runs from
  `${workspaceFolder}/app`, and opens
  `http://localhost:%s/index.html`.
- Start the server with `python3 -m http.server 5500` from `app/`, request
  `/index.html`, and confirm the document, stylesheet, and JSON data are
  served successfully.
- Load the page through `http://localhost:5500/index.html` and confirm that
  project content renders from the JSON array, rather than being hard-coded
  into the page.
- Exercise loading, populated, empty, malformed, and fetch-failure states
  where the implementation supports them.
- Check keyboard navigation, focus visibility, semantic structure, contrast,
  responsive layouts, and behavior at increased zoom.
- Confirm the direct launch URL opens the dashboard document and never a
  directory listing.
