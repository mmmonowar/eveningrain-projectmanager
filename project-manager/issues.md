# Issues

## BUG-001: baseURL returns 404 Not Found

- **Status**: Open
- **Severity**: High
- **Description**: The site's baseURL (`https://mmmonowar.github.io/eveningrain/`) returns a 404 Not Found error.
- **Root Cause**: GitHub Pages is not enabled in the `mmmonowar/eveningrain` repo settings.
- **Fix**: In the `mmmonowar/eveningrain` repo on GitHub, go to Settings > Pages > Source and select "GitHub Actions".

## BUG-002: GitHub Actions workflow fails on content repo checkout

- **Status**: Open
- **Severity**: High
- **Description**: The `Checkout content` step in `.github/workflows/hugo.yml` fails with: `Error: Not Found - https://docs.github.com/rest/repos/repos#get-a-repository`
- **Root Cause**: The workflow references `mmmonowar/eveningrain-contents` via `actions/checkout@v4`, but the repository likely does not exist on GitHub yet at `https://github.com/mmmonowar/eveningrain-contents`.
- **Dependency**: Both repos must exist and be pushed before the workflow can succeed.
- **Fix**: Create the `mmmonowar/eveningrain-contents` repository on GitHub (public), then push the local content using GitHub Desktop.
