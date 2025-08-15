# 🌌 Golden Crow Web — Pocket Genes (Astro)

Pocket Genes is a fast, static site built with Astro that showcases our mobile genomic app.  
This repo is organized with the Astro project inside the `pocket-genes/` subfolder and deploys automatically to **GitHub Pages**.

Live site: https://federico0812.github.io/golden-crow-web/

---

## 🖼 Preview

<p align="center">
  <img src="pocket-genes/public/more1.png" alt="More 1" width="200" />
  <img src="pocket-genes/public/more2.png" alt="More 2" width="200" />
  <img src="pocket-genes/public/more3.png" alt="More 3" width="200" />
</p>
<p align="center">
  <img src="pocket-genes/public/more4.png" alt="More 4" width="200" />
  <img src="pocket-genes/public/more5.png" alt="More 5" width="200" />
  <img src="pocket-genes/public/more6.png" alt="More 6" width="200" />
</p>

---

## 🛠 Prerequisites

- Node.js 18 or newer (check with: node -v)
- npm (check with: npm -v)

---

## 🧞 Quick Start (Local Development)

1) Clone and enter the project  
git clone https://github.com/Federico0812/golden-crow-web.git  
cd golden-crow-web/pocket-genes  

2) Install dependencies  
npm install  

3) Run the dev server  
npm run dev  
Open http://localhost:4321  

4) Build for production  
npm run build  
Output goes to pocket-genes/dist  

5) Preview the production build  
npm run preview  

To stop any running server: press Ctrl + C in the terminal.

---

## 📂 Project Structure (inside pocket-genes/)

    pocket-genes/
    ├─ public/                 # Static files copied to the site root
    │  ├─ mobile1.png
    │  ├─ mobile2.png
    │  ├─ aboutus1.png
    │  ├─ experience1.png
    │  └─ favicon.svg
    ├─ src/
    │  ├─ layouts/
    │  │  ├─ Hero.astro
    │  │  ├─ AboutUs.astro
    │  │  └─ Experience.astro
    │  └─ pages/
    │     └─ index.astro
    ├─ astro.config.mjs
    ├─ package.json
    └─ tsconfig.json

Notes:
- Files in `public/` are served from the site root; reference them as `/mobile1.png` or `mobile1.png` in `.astro` files.
- Main image references currently live in:
  - `src/layouts/Hero.astro`
  - `src/layouts/AboutUs.astro` (multiple)
  - `src/layouts/Experience.astro` (via ExperienceCard)

---

## 🧰 Useful Commands (run in pocket-genes/)

| Command           | Description                                       |
| ----------------- | ------------------------------------------------- |
| npm install       | Install dependencies                              |
| npm run dev       | Start dev server at http://localhost:4321         |
| npm run build     | Build to ./dist                                   |
| npm run preview   | Preview the built site locally                    |
| npm run astro -- --help | Astro CLI help                             |

---

## 🌍 Deploying to GitHub Pages (CI/CD)

This repository deploys via GitHub Actions from the `main` branch. Because the Astro app sits in `pocket-genes/`, the workflow runs there and then uploads `pocket-genes/dist`.

1) Confirm Astro base path for Pages subpath

Open `pocket-genes/astro.config.mjs` and ensure:

    import { defineConfig } from 'astro/config';
    export default defineConfig({
      base: '/golden-crow-web/',
      output: 'static',
    });

2) Ensure workflow exists at repo root: `.github/workflows/deploy.yml`

    name: Deploy to GitHub Pages

    on:
      push:
        branches: [main]

    permissions:
      contents: read
      pages: write
      id-token: write

    defaults:
      run:
        working-directory: pocket-genes

    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout source code
            uses: actions/checkout@v4

          - name: Setup Node.js
            uses: actions/setup-node@v4
            with:
              node-version: 18

          - name: Install dependencies
            run: npm install

          - name: Build Astro site
            run: npm run build

          - name: Upload artifact
            uses: actions/upload-pages-artifact@v3
            with:
              path: pocket-genes/dist

          - name: Deploy to GitHub Pages
            uses: actions/deploy-pages@v4

3) In GitHub → Settings → Pages
   - Build and deployment → Source: GitHub Actions

4) Deploy
   - Commit and push to `main`
   - Watch the run at: https://github.com/Federico0812/golden-crow-web/actions
   - When green, your site updates at: https://federico0812.github.io/golden-crow-web/

---

## 🧩 Customizing Images

To use different hero/section images, edit these files:

- `src/layouts/Hero.astro` (two images: classes `img1`, `img2`)
- `src/layouts/AboutUs.astro` (four image slots)
- `src/layouts/Experience.astro` (images passed to `<ExperienceCard ... src="...">`)

Example change in Hero.astro:

    <div class="hero-images">
      <img src="mobile3.png" alt="Pocket Genes App" class="img1" />
      <img src="mobile4.png" alt="Pocket Genes Mobile" class="img2" />
    </div>

Make sure the referenced files exist under `public/`.

---

## 🧹 Browser Cache Gotcha

If you’ve deployed but still see old images/text:
- Hard refresh the page:
  - macOS: Cmd + Shift + R
  - Windows: Ctrl + Shift + R
- Or open the site in a private/incognito window.

---

## 🧪 Troubleshooting CI

- Workflow fails on “npm ci”
  - Switch to `npm install` (already done above) or commit `package-lock.json`.

- Workflow runs but site shows old version
  - The deploy finished, but your browser cached assets. See “Browser Cache Gotcha”.

- Images 404 on Pages
  - Ensure they live in `pocket-genes/public/` and paths in `.astro` are correct (e.g., `/mobile2.png`).

- Workflow not detected
  - Confirm the file path is `.github/workflows/deploy.yml` at the **repository root** (not inside `pocket-genes/`).

---

## 📚 Reference

This site started from the Astro “Basics” template.

Create a new Astro basics project:
    npm create astro@latest -- --template basics

Astro docs:
- https://docs.astro.build
- Deployment to GitHub Pages: https://docs.astro.build/en/guides/deploy/github/

---

© Golden Crow VS — Pocket Genes
