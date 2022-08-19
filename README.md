# check-ghpages-versions
This repo, and the **Daniel Ridge** account that hosts it, function together as an automated checker bot for the [ghpages-docker image](https://github.com/hackforla/ghpages-docker) created and used by [Hack for LA](https://www.hackforla.org). 

`ghpages-docker` is designed to mimic the GitHub Pages environment on a local server, and is thus pegged to the dependency versions used by GitHub Pages. When those change, the `ghpages-docker` Dockerfile needs to be updated, and a new image built from it and pushed to Docker Hub. The workflows in this repo periodically check those dependencies and, when GitHub Pages upgrades to a new version, open an issue in the [hackforla/ops repo](https://github.com/hackforla/ops).

Specifically, there are two dependencies that need to be checked: **Ruby** and the **`github-pages` Ruby gem**. There is one workflow for each, entitled `check-ghpages-version.yml` and `check-ruby-version.yml`, respectively. The code for both is nearly identical, but the `check-ghpages-version` workflow runs every day at 1200 UTC, and the `check-ruby-version` workflow is triggered upon the completion of `check-ghpages-version`.

Each workflow functions by reading the version number for its respective piece of sotware from the JSON version of [GitHub's dependency version list](https://pages.github.com/versions.json) and comparing it to the version number currently being used by `ghpages-docker`, which is stored in a text file in the `release-versions` folder in the root of the repo. If there is a change, the workflow opens an issue in `hackforla/ops` notifying the Ops team that an update is required. It also writes the new version number to the appropriate text file in `release-versions` and commits the change. Note that this means that **by the time an issue is opened, the version number in the relevant text file already reflects the new version used by GitHub Pages, not the version currently used in `ghpages-docker`**.

Workflows and README written by [ericvennemeyer](https://github.com/ericvennemeyer) for [hackforla](https://github.com/hackforla)
