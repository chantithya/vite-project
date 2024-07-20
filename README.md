# Vite + React + Typescript

[![N|Solid](https://media.dev.to/cdn-cgi/image/width=1000,height=420,fit=cover,gravity=auto,format=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fitchfnij5b0g1k4es7wm.png)](https://nodesource.com/products/nsolid)

## 0.1 create vite react app 
```js
npm create vite@latest
```

## 0.2 Create a new repository on GitHub and initialize GIT
```js
git init 
git add . 
git commit -m "add: initial files" 
git branch -M main 
git remote add origin https://github.com/[USER]/[REPO_NAME] 
git push -u origin main
```

## 0.3 Setup base in vite.config
```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  base: "/[REPO_NAME]/",
})
```

## 0.4 Create .github/workflows/deploy.yml and add the code bellow
> ⚠️ Warning
> It is crucial that the `.yml` file has the exact code below. Any typing or spacing errors may cause deployment issues.
```js
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repo
        uses: actions/checkout@v2

      - name: Setup Node
        uses: actions/setup-node@v1
        with:
          node-version: 16

      - name: Install dependencies
        uses: bahmutov/npm-install@v1

      - name: Build project
        run: npm run build

      - name: Upload production-ready build files
        uses: actions/upload-artifact@v2
        with:
          name: production-files
          path: ./dist

  deploy:
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v2
        with:
          name: production-files
          path: ./dist

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```
## 05. Push to GitHub
```js
git add . 
git commit -m "add: deploy workflow" 
git push
```

## 06. Active workflow (GitHub)
```js
Config > Actions > General > Workflow permissions > Read and Write permissions 
```
```js
Actions > failed deploy > re-run-job failed jobs 
```
```js
Pages > gh-pages > save
```

## 🛠 Helper
For code changes
Whenever you push to GitHub, it will deploy automatically.
```js
git add . 
git commit -m "fix: some bug" 
git push
```
