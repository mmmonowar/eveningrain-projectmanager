# Issues

## BUG-001: baseURL returns 404 Not Found

- **Status**: Resolved
- **Severity**: High
- **Resolution**: GitHub Pages enabled in repo settings under Source > GitHub Actions.
- **Description**: The site's baseURL (`https://mmmonowar.github.io/eveningrain/`) returns a 404 Not Found error.
- **Root Cause**: GitHub Pages is not enabled in the `mmmonowar/eveningrain` repo settings.
- **Fix**: In the `mmmonowar/eveningrain` repo on GitHub, go to Settings > Pages > Source and select "GitHub Actions".

## BUG-002: GitHub Actions workflow fails on content repo checkout

- **Status**: Resolved
- **Severity**: High
- **Resolution**: Changed from `actions/checkout@v4` with `repository:` parameter to a plain `git clone --depth=1` bash step, which bypasses the GitHub API and works for any public repo.
- **Description**: The `Checkout content` step in `.github/workflows/hugo.yml` fails with: `Error: Not Found - https://docs.github.com/rest/repos/repos#get-a-repository`
- **Root Cause**: The default `GITHUB_TOKEN` in the runner cannot resolve external repos via the GitHub API, even for public repos, when using `actions/checkout@v4` with `repository:`.
- **Fix**: Replace `actions/checkout@v4` with `git clone --depth=1` in the workflow.

## BUG-003: Live site shows no posts on homepage and search

- **Status**: Open
- **Severity**: High
- **Description**: The live site loads at `https://mmmonowar.github.io/eveningrain/` but the homepage shows an empty recent posts section and the search index (`search.json`) is empty (`[]`). The post pages (`/dystopia/`) and archive (`/posts/`) render correctly with content.
- **Root Cause**: The site was deployed from a mixed build state. An earlier workflow run (without the BUG-002 fix) deployed an empty homepage and search index. A later run (with the `git clone` fix) deployed the content pages, but GitHub Pages replaced only changed files, leaving the old empty homepage and search index in place.
- **Fix**: Trigger a fresh full build and deploy by pushing a trivial commit to the `eveningrain` repo (e.g. update a comment in a layout file) to force the workflow to rebuild and replace all files.
