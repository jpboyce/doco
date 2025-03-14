---
title: How This Site Was Built
date: 2025-03-14
categories:
    - General
tags:
    - MkDocs
    - Material for MkDocs
    - HOWTO
---    
# How This Site was Built

This site uses [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and [Github Pages](https://pages.github.com/).

Material for MkDocs is a documentation framework that sits on to of [MkDocs](https://www.mkdocs.org/).  It allows documentation to be written in Markdown, which is then presented in a professional way.

Github Pages allows the hosting of static content sites from a GitHub on Github.  This is a free service.

By using these two products, it's possible to implement "documentation-as-code" in a relatively easy to use fashion that presents well-formatted documentation.

## Setup

### Github Repository

1. Go to Github and create a new repository

2. Clone the repository to your local system

### Installing Material for MkDocs

1. Install Python
2. Install pip
3. Install Material for MkDocs using pip: `pip install mkdocs-material`
4. In the directory containing the cloned repository, run the command to create a new MkDocs project: `mkdocs new .`
5. Update the mkdocs.yml file, adding the site name, URL and the theme settings
``` yml
site_name: Jesse's Doco
site_url: https://doco.jpboyce.org
theme:
  name: material
```
6. At this point, it's possible to run the development server and view the site locally: `mkdocs serve`

### Publishing with GitHub Actions
1. In the repository, create a workflow file.  This workflow will run on any commits pushed to the main or master branch, running the command to deploy the site.  The site contents will be published to a separate branch called `gh-pages`
``` yml title=".github/workflows/ci.yml"
name: ci
on:
  push:
    branches:
      - master
      - main
permissions:
  contents: write
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Configure Git Credentials
        run: |
          git config user.name github-actions[bot]
          git config user.email 41898282+github-actions[bot]@users.noreply.github.com
      - uses: actions/setup-python@v5
        with:
          python-version: 3.x
      - run: echo "cache_id=$(date --utc '+%V')" >> $GITHUB_ENV
      - uses: actions/cache@v4
        with:
          key: mkdocs-material-${{ env.cache_id }}
          path: .cache
          restore-keys: |
            mkdocs-material-
      - run: pip install mkdocs-material
      - run: mkdocs gh-deploy --force
```
2. Commit the workflow file and any others to the repository
3. Navigate to the Actions tab on the repository to confirm the workflow is running and completes successfully
4. After the workflow completes, the 2nd branch `gh-pages` should be visible
5. Navigate to the Repository settings
6. Click on Pages
7. Set the branch to `gh-pages` and click Save
8. Enter the custom domain for the site and click Save
9. Go to the DNS provider for the custom domain and add a CNAME record for the custom domain that points to `<user>.github.io`
10. Go back to the Pages section for the repository.  It should regularly perform a DNS check.  Once that passes, it will start provisioning a TLS certificate.
!!! info
    The certificate provisioning process can take up to 15 minutes
12. Once the certificate has been provisioned, click the Enforce HTTP checkbox so the site is accessable in a secure fashion    

