# self

My public obsidian vault.

## Setup

I followed advice found [here](https://oliverfalvai.com/evergreen/my-quartz-+-obsidian-note-publishing-setup) which involves a git subtree of the quartz project, instead of forking and modifying from there.

### Subtree Command

```sh
git subtree add --prefix=quartz https://github.com/jackyzha0/quartz.git v4 --squash
```

#### Setup Quartz

Install and set the build directory to the vault above.

```
npm i
npx quartz build --directory=../self
```

> [!NOTE]
> Obsidian stuff...

### Run the server

```sh
npx quartz build --serve --directory=../self
```

### Github Workflow

```yaml
name: Deploy using Quartz and Github Pages
 
on:
  push:
    branches:
      - main
 
permissions:
  contents: read
  pages: write
  id-token: write
 
concurrency:
  group: "pages"
  cancel-in-progress: false
 
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build --directory=../self
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: public
 
  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

```
