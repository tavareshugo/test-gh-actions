# Quarto Book Archive Release Action

This action builds and archives tagged versions of a Quarto book into the `gh-pages` branch of your repo.

This action should ideally be paired with the [Quarto Deploy Action](../deploy) to publish the latest version of your book.

## What it does

When you push a tag to your repository matching the `YYYY.MM.DD` scheme, this action:

- Checks out the tagged commit.
- Extracts the tag name (e.g. 2025.01.01).
- Sets up Quarto.
- Optionally runs user-defined pre-render commands (e.g. if you need to install other software dependencies).
- Runs quarto render.
- Optionally runs post-render commands.
- Moves the rendered `_site` to a directory named after the tag.
- Checks out the `gh-pages` branch into `_site`.
- Copies the new archive into `_site/archive/`.
- Commits and pushes changes back to the `gh-pages` branch.

**The result**: each tagged version of your book is published to `gh-pages/archive/<tag>/`.

## Usage

In your Quarto repository, create a workflow file (e.g. `.github/workflows/archive.yml`), which runs both this action as well as the [deploy action](../deploy):

```yaml
name: Tag Release

on:
  push:
    tags:
      - '20[2-9][0-9].[0-1][0-9].[0-3][0-9]'

jobs:
  archive:
    runs-on: ubuntu-latest
    steps: 
      - name: Archive
        uses: cambiotraining/crit-gh-actions/course-archive@main
        with:
          pre_render: "echo 'Optional pre-render commands'"
          post_render: "echo 'Optional post-render commands'"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

  deploy:
    runs-on: ubuntu-latest
    needs: archive
    steps:
      - name: Deploy
        uses: cambiotraining/crit-gh-actions/course-deploy@main
        with:
          pre_render: "echo 'Optional pre-render commands'"
          post_render: "echo 'Optional post-render commands'"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Example Usage

If you tag a commit following the `YYYY.MM.DD` format (e.g. `2025.01.01`), the action will build and archive that version of your Quarto book:

```bash
git tag 2025.01.01
git push origin 2025.01.01
```

This will result in the rendered book being available at `https://<your-username>.github.io/<your-repo>/archive/2025.01.01/`.

When you pair this with the [deploy action](../deploy), the latest version of your book will be available at the root URL, with a dropdown added to the navigation bar taking you to the archived versions.