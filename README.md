# jekyll-build-tools

This repository helps with maintaining jekyll-based documentation websites
across the Alfresco org.

## Quick start

Use the contents of the [docs](./docs) as a starter template for your
repository, copying over the files into it.

If you already have some markdown files hanging around, make sure to move all
them under the same `docs` folder and add a [front matter](https://jekyllrb.com/docs/front-matter/)
to each of them at the beginning of the file:

```yml
---
title: Page title
---
```

Then create into your repository a new workflow which will take care of building
and publishing the website, e.g. `.github/workflows/docs.yml`:

```yml
name: Publish docs

permissions:
  contents: read
  pages: write
  id-token: write

on:
  push:
    branches:
      - main
    paths:
      - "docs/**"
      - ".github/workflows/docs.yml"
  pull_request:
    branches:
      - main
    paths:
      - "docs/**"
      - ".github/workflows/docs.yml"

jobs:
  publish:
    uses: Alfresco/jekyll-build-tools/.github/workflows/jekyll-publish.yml@main
    with:
      working-directory: docs
      # when to trigger the publish job
      publish: ${{ github.event_name == 'push' && github.ref_name == 'main'}}
```

## Local preview

You can also preview your website locally.

A Ruby runtime (e.g.[rbenv](https://github.com/rbenv/rbenv)) is mandatory.

First time setup:

```sh
# enter your project doc folder
cd docs
# install the ruby dependency manager
gem install bundler
# install dependencies using the Gemfile.lock
bundle install
```

Finally run it locally:

```sh
bundle exec jekyll serve -l
```
