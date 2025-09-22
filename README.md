# CRIT GitHub actions

Actions for use with CRIT course materials.
There are two main actions: 

- Quarto book deployment: deploys the latest version of a quarto book to GitHub pages, adding a dropdown menu to select previous versions.
- Quarto book archive release: adds rendered files of a new tagged version to the archive directory of the `gh-pages` branch. This directory is used by the dropdown menu added by the deployment action.