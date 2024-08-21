# Deploying a Vite + React App to GitHub Pages with Custom Domain
This tutorial is derived from `https://github.com/gitname/react-gh-pages`.
To view this Vite + React app as a github page vist (https://cesardgm.github.io/my-vite-app)[https://cesardgm.github.io/my-vite-app].

## Prerequisites

1. Node.js and npm (Node Package Manager)
   - Recommended versions: Node.js v20.11.1 or later, npm 10.2.4 or later
2. Git (version 2.34.1 or later)
3. GitHub account

## Creating a GitHub Repository

1. Log in to your GitHub account.
2. Create a new public repository to host your project's source code.
3. For this guide, we'll use `my-vite-app` as the repository name. Replace this with your preferred name.

## Deploying a Vite + React App to GitHub Pages

1. Create a new Vite app:
   ```
   npm create vite@latest my-vite-app
   ```
   - Select `React` as the framework
   - Choose `TypeScript + SWC` as the variant

2. Navigate to the project directory and install dependencies:
   ```
   cd my-vite-app
   npm install
   ```

3. Install the `gh-pages` package as a development dependency:
   ```
   npm install gh-pages --save-dev
   ```

4. Update the `package.json` file:
   - Add a `homepage` property:
     ```json
     {
       "name": "my-vite-app",
       "version": "0.0.0",
       "homepage": "https://{username}.github.io/my-vite-app",
       "private": true,
     ```
   - Add deployment scripts:
     ```json
     "scripts": {
       "predeploy": "npm run build",
       "deploy": "gh-pages -d build",
       "dev": "vite",
       "build": "tsc -b && vite build",
       "lint": "eslint .",
       "preview": "vite preview"
     },
     ```

5. Modify the `vite.config.ts` file:
   ```typescript
   import { defineConfig } from 'vite';
   import react from '@vitejs/plugin-react-swc';

   export default defineConfig({
     plugins: [react()],
     build: {
     outDir: 'build',
     },
     base: "/my-vite-app/",
   });
   ```

6. Initialize Git and add the remote repository:
   ```
   git init
   git remote add origin git@github.com:{username}/my-vite-app.git
   ```

7. Build and deploy the app:
   ```
   npm run build
   npm run deploy
   ```

8. Configure GitHub Pages:
   - Go to your repository on GitHub
   - Navigate to Settings > Pages
   - Set "Source" to "gh-pages" branch, folder "/ (root)"
   - Click "Save"

9. Your app is now live at `https://{username}.github.io/my-vite-app`

10. Push the source code to GitHub:
    ```
    git add .
    git commit -m "Initial commit"
    git push -u origin main
    ```

## Notes

- Replace `{username}` with your GitHub username throughout this guide.
- Ensure your repository is public, or upgrade to GitHub Pro for private repository support.
- The `base` property in `vite.config.ts` should match your repository name.
- Clear the cache in your browser to ensure you are loading the latest deployment.

By following these steps, you'll successfully deploy your Vite + React app to GitHub Pages with a custom domain.

## Configuring AWS Route 53 Domain with GitHub Pages

1. Prepare a `favicon.ico` file and place it at the root level of your repository.

2. Domain Registration and DNS Configuration:
   - Purchase a domain through AWS Route 53.
   - Navigate to the hosted zone for your purchased domain.
   - Don't delete any of the default records.
   - Create a `Simple Record` of type `A`:
     - Set `Alias` to No
     - Enter the following IP addresses for `Value/Route traffic to`:
       ```
       185.199.108.153
       185.199.109.153
       185.199.110.153
       185.199.111.153
       ```

3. GitHub Domain Verification:
   - Go to your GitHub account settings > Pages (under *Code, planning, and automation*).
   - Click "Add a verified domain" and enter your AWS Route 53 domain.
   - Copy the provided TXT record subdomain and value.
   - In AWS Route 53, create a TXT record:
     - Subdomain: Replace `www` with the GitHub-provided value (typically `_github-pages-challenge-{username}`)
     - Value: Paste the GitHub-provided code
   - Create a CNAME record:
     - Type: CNAME
     - Value/Route traffic to: `{username}.github.io`
   - Return to GitHub and click "Verify".

4. Local Repository Configuration:
   - Create a file named `CNAME` (without any extension) in the root directory of your local repository.
   - Add a single line to the file: `www.{customDomain}.{extension}`

5. Update Vite Configuration:
   - Modify `vite.config.ts`:
     - Change the `base` property from `/my-vite-app/` to `/`

6. Rebuild and Redeploy:
   ```
   npm run build
   npm run deploy
   ```

7. GitHub Repository Settings:
   - Navigate to your repository's settings > Pages (under *Code and automation*).
   - Under "Custom domain", enter your verified domain.
   - Click "Save" and enable "Enforce HTTPS".

8. Completion:
   Your Vite + React app is now deployed and accessible via your custom domain with HTTPS enforced.

## Notes:
- Replace `{username}` with your GitHub username.
- Replace `{customDomain}` and `{extension}` with your actual domain name and top-level domain.
- Ensure all DNS changes are propagated, which may take up to 48 hours.
- Regularly check and renew your domain registration and SSL certificate to maintain uninterrupted service.

By following these steps, you'll successfully configure your AWS Route 53 domain with GitHub Pages, providing a professional and secure hosting solution for your Vite + React application.
