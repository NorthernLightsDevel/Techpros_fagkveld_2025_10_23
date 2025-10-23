# Repository Guidelines

## Project Structure & Module Organization
`Fagkveld_2025_Oktober.md` is the single source of truth for the slide deck. Preserve the YAML front matter—it controls theme, pagination, and shared CSS (see the existing `style:` block). The exported `Fagkveld_2025_Oktober.html` is committed for quick preview; regenerate it whenever the Markdown changes. If you add PDF or PPTX exports, keep them in the repo only when stakeholders require versioned artefacts. Store new images or fonts under `assets/` and reference them with relative paths so Marp bundles them correctly.

## Build, Test, and Development Commands
Install the CLI via `npm install --global @marp-team/marp-cli`, or run it on demand with `npx @marp-team/marp-cli`. Use `marp --preview Fagkveld_2025_Oktober.md` while editing for live reload in the browser. Refresh the checked-in HTML with `marp Fagkveld_2025_Oktober.md -o Fagkveld_2025_Oktober.html`. Produce deliverables through `marp Fagkveld_2025_Oktober.md --pdf` and `marp Fagkveld_2025_Oktober.md --pptx` before sharing slides.

## Coding Style & Naming Conventions
Write one logical slide between each `---` separator and keep titles at H1/H2 level. Use semantic Markdown first; add HTML comments (e.g. `<!-- _class: lead -->`) only for layout tweaks. Fence code or commands with language hints for syntax highlighting. Keep custom CSS minimal, kebab-case any helper classes, and document unusual choices inline so co-authors can follow them.

## Testing Guidelines
Automated tests are not present, so perform manual checks. Preview with `marp --preview` to catch layout issues, export to HTML/PDF to confirm assets resolve, and exercise presenter notes in the browser. Before delivery, open the PDF in Acrobat or Preview to validate pagination and embedded fonts, and smoke-test the HTML deck in Chromium and Firefox to guard against theme regressions.

## Commit & Pull Request Guidelines
Prefer concise, imperative subjects (`Tighten agenda slide bullets`). Group related slide revisions together and describe major narrative or design changes in the commit body. Pull requests should explain the goal, link to updated exports or screenshots, and note which formats were regenerated. Tag reviewers responsible for the session content and ensure exported artefacts are up to date before requesting approval.

## Accessibility & Asset Hygiene
Provide alt text for every image, maintain high contrast, and avoid relying solely on colour to convey meaning. Compress large media before committing, and keep sensitive demo data outside the repository. Limit code snippets to chunks that stay legible from the back row.
