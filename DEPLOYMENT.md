# Deployment Workflow

This project is separated into two repositories to support GitHub Pages deployment:
1. **Source Code Repository (`precisionjs-home-src`)**: Contains the Astro project, Vue components, and Tailwind configuration.
2. **Deployment Repository (`precisionjs-home`)**: Contains only the built static assets that are served by GitHub Pages.

## How to Deploy

To deploy new changes to `precision.js.org`:

1. Build the Astro project locally inside the source repository (`precisionjs-home-src`):
   ```bash
   npm run build
   ```
   This will generate the static files inside the `dist/` directory.

2. Ensure you have the deployment repository cloned in a sibling directory called `precisionjs-home-build`:
   ```bash
   mkdir -p ../precisionjs-home-build
   cd ../precisionjs-home-build
   # If not already cloned:
   git clone https://github.com/jko206/precisionjs-home.git
   ```

3. Copy the built artifacts to the deployment repository. Using `rsync` is recommended so old deleted files are also removed from the deployment repo, while preserving the `.git` directory:
   ```bash
   rsync -av --exclude='.git/' --delete ../precisionjs-home/dist/ ./precisionjs-home/
   ```

4. Commit and push the changes in the deployment repository:
   ```bash
   cd precisionjs-home
   git add .
   git commit -m "Deploy latest build"
   git push origin master
   ```

GitHub Pages will automatically pick up the new commits on the `master` branch of the `precisionjs-home` repository and deploy them to `precision.js.org`.
