# CRIT GitHub Actions

Actions for use with CRIT course materials.
There are two main actions:

- [Course deployment](./course-deploy): deploys the latest version of a quarto book to GitHub pages, adding a dropdown menu to select previous versions.
- [Course archive release](./course-archive): adds rendered files of a new tagged version to the archive directory of the `gh-pages` branch. This directory is used by the dropdown menu added by the deployment action.
