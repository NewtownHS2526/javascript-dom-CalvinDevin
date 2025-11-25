This repository is a static JavaScript/DOM exercises site used for classroom assignments.

- Purpose: provide small, self-contained exercises under `exercises/` where students edit a local `script.js`, run `index.html` in a browser, and verify output via the DevTools Console.

- Key files:
  - `index.html`: top-level page that loads scripts and styles for exercises.
  - `script.js`: root example script; many exercises include their own `script.js` in the exercise folder.
  - `style.css`: page styling.
  - `exercises/<topic>/level-XX/exercise-Y/`: each exercise folder contains `README.md`, a template `script.js`, and often an `example.js` with a solved version.
  - `ai-review.mjs`: small Node script used in the classroom CI to post an automated AI review of HTML/CSS/JS PR patches (requires `OPENAI_API_KEY`, `PR_NUMBER`, `GITHUB_REPOSITORY` environment variables when run outside CI).

- Project specifics agents must know:
  - Exercises are independent: changes should be localized to the specific `exercises/.../exercise-*/` folder unless asked to modify shared files (`index.html`, `style.css`).
  - Follow the exercise `README.md` and the `TODO` comments inside that exercise's `script.js`.
  - Many exercises include an `example.js` that demonstrates the expected solution — use it as a reference but do not overwrite it.

- Development / verification workflow (no build system):
  - Open `index.html` in a browser and use Developer Tools (F12) → Console to verify output.
  - Quick command (PowerShell) to open the page from repo root:

    ```powershell
    ii .\index.html
    ```

  - To run the AI review script locally (for debugging): set env vars then run with Node. Example (PowerShell):

    ```powershell
    $env:OPENAI_API_KEY = 'sk-...'
    $env:PR_NUMBER = '1'
    $env:GITHUB_REPOSITORY = 'owner/repo'
    node .\ai-review.mjs
    ```

- Conventions & patterns to follow when editing code:
  - Keep changes minimal and localized to an exercise folder unless the task explicitly requires cross-exercise updates.
  - Preserve `README.md` instructions and `example.js` files; implement solutions in the exercise `script.js` template.
  - Prefer plain, readable JavaScript. These exercises target learners — aim for clarity over cleverness.
  - Search for `TODO` comments to find the intended edit points. Example search (PowerShell):

    ```powershell
    Select-String -Path .\exercises\**\*.js -Pattern "TODO" -List
    ```

- CI / integration notes:
  - `ai-review.mjs` runs in the classroom CI environment and early-exits when `PR_NUMBER` or `GITHUB_REPOSITORY` are missing.
  - It expects to find patch text from PR files and will only analyze `.html`, `.css`, and `.js` files (first 10 matching files).
  - Do not rely on the script for blocking correctness — it produces educational feedback only.

- When generating edits or PRs:
  - Include a short summary referencing the exercise path (for example: "Solve `exercises/01-variables-data-types/level-04/exercise-1/script.js` TODO").
  - Keep diffs small and focused so the automated reviewer and instructors can give targeted feedback.

- Examples from this repo:
  - `exercises/01-variables-data-types/level-04/exercise-1/README.md` shows the typical exercise layout and verification steps.
  - `ai-review.mjs` shows how PR changes get collected and how environment variables affect behavior.

If anything here is unclear or you want additional project-specific rules (e.g., preferred code style, whether to update `index.html` for multi-exercise demos, or how to handle `example.js`), tell me which area to expand and I will iterate.
