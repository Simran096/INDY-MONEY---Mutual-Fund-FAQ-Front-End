# Beginner guide: publish the frontend on GitHub Pages

This guide is for the separate frontend repository:
`Simran096/INDY-MONEY---Mutual-Fund-FAQ-Front-End`.

The frontend deployment workflow supports either of these layouts:

- A frontend copied into the repository root (`package.json` is at the top
  level). This is the recommended layout for the frontend-only repository.
- The full project layout with the frontend in `app/`.

## Part A: Copy the frontend into your GitHub repository

1. Install and open [GitHub Desktop](https://desktop.github.com/).
2. Sign in to GitHub Desktop.
3. In GitHub Desktop, choose **File → Clone repository**.
4. Select the **URL** tab and paste:
   `https://github.com/Simran096/INDY-MONEY---Mutual-Fund-FAQ-Front-End`
5. Choose where to save the repository on your computer and click **Clone**.
6. Open the newly cloned repository folder in File Explorer.
7. In another File Explorer window, open:
   `C:\Users\hp\Downloads\RAG Bot\app`
8. Copy the frontend project files and folders into the cloned repository's
   top level. Copy `src`, `package.json`, `package-lock.json`,
   `next.config.js`, `next-env.d.ts`, `postcss.config.js`,
   `tailwind.config.ts`, and `tsconfig.json` if present.
9. Do **not** copy `.env.local`, `node_modules`, `.next`, `out`,
   `build-output.txt`, `buildlog.txt`, or `tsconfig.tsbuildinfo`.
10. In the cloned repository, create the folder path
    `.github/workflows/`. Copy this deployment workflow into that folder as
    `deploy-pages.yml`:
    `.github/workflows/deploy-pages.yml` from the full project folder.
11. In GitHub Desktop, review the **Changes** list. Confirm `.env.local` is
    absent. Enter a summary such as `Add frontend and Pages deployment`,
    click **Commit to main**, then click **Push origin**.

The frontend folder's `next.config.js` must include static export settings
(`output: "export"`, `trailingSlash: true`, and `images.unoptimized: true`).
The project copy in this workspace is already configured that way.

## Part B: Turn on GitHub Pages

1. Open the repository page on GitHub.
2. Click **Settings**.
3. In the left menu, click **Pages**.
4. Under **Build and deployment → Source**, choose **GitHub Actions**.

## Part C: Add the Render API URL

The frontend needs the public URL of the already-deployed backend to answer
questions.

1. In the repository, go to **Settings → Secrets and variables → Actions →
   Variables**.
2. Click **New repository variable**.
3. Enter the name `NEXT_PUBLIC_API_URL`.
4. For the value, enter the Render service URL only, for example
   `https://your-service.onrender.com`. Do not add `/api` or a trailing slash.
5. Click **Add variable**.

Do not add `OPENAI_API_KEY` to GitHub. Keep it secret in Render.

If the Render backend is not deployed yet, you can still push the files and
enable Pages now. Add this variable and rerun the workflow after Render gives
you its service URL.

## Part D: Run the deployment

1. Click the repository's **Actions** tab.
2. Select **Deploy frontend to GitHub Pages** in the workflows list.
3. Click **Run workflow**, keep the branch set to `main`, then confirm **Run
   workflow**.
4. Wait for the run to finish with a green check.
5. Open **Settings → Pages** or the Actions deployment summary to find the
   published site URL.

For this repository, the expected website address is:
`https://simran096.github.io/INDY-MONEY---Mutual-Fund-FAQ-Front-End/`

## Part E: Test it

Open the website and ask a factual question. If the page loads but chat cannot
connect, confirm the GitHub `NEXT_PUBLIC_API_URL` variable is correct and the
Render service is running.

In Render, set `ALLOWED_ORIGINS` to this origin (no repo path and no trailing
slash):
`https://simran096.github.io`

After changing `NEXT_PUBLIC_API_URL`, run the GitHub Actions workflow again.
After changing Render's `ALLOWED_ORIGINS`, redeploy or restart the Render
service.
