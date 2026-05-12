# YumeShop — GitHub Deployment Files

รวมไฟล์ที่สร้างและแก้ไขเพื่อ deploy โปรเจกต์ **Yume Zen** ขึ้น GitHub Pages

---

## 1. `index.html` — [NEW] หน้า Redirect หลัก

เพิ่มเพื่อให้ GitHub Pages เปิด `shop.html` โดยอัตโนมัติเมื่อเข้า URL หลัก

```html
<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta http-equiv="refresh" content="0; url=shop.html">
<title>Yume Zen</title>
</head>
<body>
</body>
</html>
```

---

## 2. `.gitignore` — [NEW]

กรองไฟล์ที่ไม่จำเป็นออกจาก Git

```gitignore
# System files
.DS_Store
Thumbs.db
desktop.ini

# Editor folders
.vscode/
.idea/

# Logs
*.log
```

---

## 3. `.github/workflows/deploy.yml` — [NEW] GitHub Actions Workflow

Deploy อัตโนมัติเมื่อ push ขึ้น branch `main`
แก้ปัญหา Node.js 20 deprecation ด้วย FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

env:
  FORCE_JAVASCRIPT_ACTIONS_TO_NODE24: true

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Pages
        uses: actions/configure-pages@v5

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: github-pages
          path: "."

      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

---

## วิธี Push ขึ้น GitHub (หลังติดตั้ง Git)

```bash
git init
git add .
git commit -m "Initial commit - Yume Zen website"
git remote add origin https://github.com/USERNAME/yume-zen.git
git branch -M main
git push -u origin main
```

จากนั้นไปที่ GitHub → Settings → Pages → Source: GitHub Actions

เว็บจะขึ้นที่: https://USERNAME.github.io/yume-zen/
