# Debate Forge

A classifier-routed debate UI. Every question is scored for confidence first — confident ones pass straight through, uncertain ones go through a Proposer → Skeptic → Verifier panel before a Judge rules, with a consensus score and an audit trail of past runs.

This repo is a single static page (`index.html`) with no build step and no dependencies besides Google Fonts.

## Deploy with GitHub Actions (GitHub Pages)

1. Push this repo to GitHub.
2. In the repo, go to **Settings → Pages**.
3. Under **Build and deployment → Source**, choose **GitHub Actions**.
4. Push to `main` (or run the workflow manually from the **Actions** tab) — `.github/workflows/deploy.yml` will build and publish the site.
5. Your site will be live at `https://<your-username>.github.io/<repo-name>/`.

No further setup needed — the workflow uses GitHub's own `actions/configure-pages`, `actions/upload-pages-artifact`, and `actions/deploy-pages` actions.

## About the backend

The page tries to call `POST /api/debate` first (a streaming SSE endpoint) and falls back to a local, clearly-labeled demo simulation if nothing answers — which is what will happen on GitHub Pages, since it only serves static files. If you want real debate output instead of the simulation, stand up a backend that implements `/api/debate` and serve this page from the same origin (or point the fetch at your backend's URL), or use the "Run as a real debate in Claude instead" link in the page to get a copyable prompt.

The expected SSE event shape from `/api/debate`:

```
event: phase
data: {"phase":"proposing"}

event: done
data: {"answer":"...", "rounds":[{"label":"Proposer","text":"..."}, ...]}

event: error
data: {"message":"..."}
```
