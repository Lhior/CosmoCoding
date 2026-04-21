# CosmoCoding

CosmoCoding is a GitHub Pages site for cosmologists who want a practical path into modern development workflows. The site focuses on Visual Studio Code, the debugger, remote development on clusters, and tool-specific debugging patterns for common cosmology software.

## Local preview

1. Install the site dependencies:

   ```bash
   pip install -r requirements.txt
   ```

2. Start the local development server:

   ```bash
   mkdocs serve
   ```

3. Open the local URL that MkDocs prints, usually `http://127.0.0.1:8000/`.

## Deployment

The repository includes a GitHub Actions workflow that builds the site and deploys it to GitHub Pages from the `main` branch.
