# Quarto Extensions Update Action

This action updates all the Quarto extensions used in our standard CRIT course template.

It runs `quarto update` for a fixed set of extensions so that all repositories stay in sync with the latest versions.

- If you want to update other extensions, you can adapt the code from this action and create your own custom action.
- The action does not commit or push changes by itself (but see example below).

## Usage

The following shows an example workflow that runs this action on a schedule and creates a pull request with the changes.

```yaml
name: Update Quarto Extensions

on:
  schedule:
    - cron: '0 0 1 * *'   # 00:00 UTC on the 1st of each month

jobs:
  update-extensions:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Update Quarto extensions
        uses: cambiotraining/crit-quarto-extensions/update-crit-extensions@main

      - name: Create PR with updates
        uses: peter-evans/create-pull-request@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          commit-message: "chore: automated update to Quarto extensions"
          branch: "update-extensions"
          title: "chore: automated update to Quarto extensions"
```
