# Profile README and Contribution Snake Workflow Design

## Goal

Keep the existing GitHub profile README layout while making it belong to
`Pocon041` and adding an automated contribution snake workflow.

## Scope

- Add `.github/workflows/snake.yml`.
- Generate light and dark contribution snake SVG files with `Platane/snk@v3`.
- Publish generated SVG files to an orphan `output` branch.
- Run the workflow daily, when `main` changes, and when manually triggered.
- Replace stale `bigorange18` references in `README.md` with `Pocon041`.
- Replace visible mojibake in headings, badge labels, image alternative text,
  and comments while preserving the current layout and social links.
- Point the README contribution snake images at the generated files on the
  `output` branch.

## Workflow Design

The workflow will use `actions/checkout@v4`, generate both snake SVG variants,
and publish the generated `dist` directory with
`crazy-max/ghaction-github-pages@v4.2.0`.

Triggers:

- `schedule`: once per day.
- `workflow_dispatch`: manual runs from the GitHub Actions page.
- `push`: updates to `main`.

The publish job requires `contents: write` permission so it can update the
`output` branch.

## README Changes

The profile keeps its existing sections and external social links. The update
is intentionally narrow:

- Use `Pocon041` for GitHub statistics widgets and visitor counters.
- Read contribution snake SVG files from the repository's `output` branch.
- Replace broken visible encoding artifacts with readable English labels.
- Remove the stale copied file header because it identifies a different
  template author and adds no profile value.

## Error Handling

If generation or publication fails, the workflow job fails visibly in GitHub
Actions. The previously published SVG files remain available from the
`output` branch, so the profile README continues to render its last successful
version.

## Verification

- Parse `.github/workflows/snake.yml` as YAML.
- Confirm `README.md` no longer contains `bigorange18`.
- Confirm README snake image URLs reference the `output` branch.
- Inspect the final diff to ensure the existing page structure and social
  links remain intact.
