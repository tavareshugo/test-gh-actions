# Quarto Book Deployment Action

Render the `main` branch of a Quarto book, merge in any existing archived versions (see [archive action](../archive)), inject the version dropdown into the navbar, and deploy the final site to the `gh-pages` branch.

## What it does

This action:

- Checks out main.
- Sets up Quarto.
- Optionally runs user-defined pre-render commands (e.g. if you need to install other software dependencies).
- Runs quarto render.
- Checks out the existing gh-pages (sparse-checkout of the `archive` folder).
- Copies existing `archive/` content into `_site/archive/`.
- Runs the `update-dropdown.py` script (see note below) to inject a Versions dropdown and update `versions.html` appendix.
- Deploys `_site` to `gh-pages`.

## Usage

### Deploy latest version

Here is an example workflow file (e.g. `.github/workflows/deploy.yml`) that triggers when pushing to the `main` branch:

```yaml
name: Deploy Latest

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Run Quarto Deployment Action
        uses: cambiotraining/crit-gh-actions/course-deploy@main
        with:
          pre_render: "echo 'Optional pre-render commands'"
          post_render: "echo 'Optional post-render commands'"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

This ensures the latest version of the book is always deployed to `gh-pages`.

### Archive versions

You can also pair this action with the [archive action](../archive) to automatically add archived versions of your book.
See the [archive README](../archive/README.md) for details.
