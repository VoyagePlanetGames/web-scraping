# Publish to GitHub Pages

Run these from a terminal **on your Mac**, inside this folder
(`D93/web-scraping`). They push the site to your repo and make it public online.

```bash
cd "D93/web-scraping"

# 1. Remove the partial .git left by the sandbox (your Mac can delete it fine)
rm -rf .git

# 2. Fresh repo + first commit
git init
git add -A
git commit -m "Day 93: multi-source web scraper with CSV export + Buy Me a Coffee"
git branch -M main

# 3. Connect your repo and push (use the SSH or HTTPS remote you prefer)
git remote add origin https://github.com/VoyagePlanetGames/web-scraping.git
git push -u origin main --force   # --force only needed if the repo already has commits
```

## Turn on GitHub Pages (makes it accessible online)

1. Go to **https://github.com/VoyagePlanetGames/web-scraping/settings/pages**
2. Under **Source**, choose **Deploy from a branch**.
3. Branch: **main**, folder: **/ (root)** → **Save**.
4. Wait ~1 minute. Your live URL will be:
   **https://voyageplanetgames.github.io/web-scraping/**

## Add your photo (optional)
Drop a square `profile.jpg` into the `assets/` folder before pushing, and it
shows in the header. Until then a built-in initial avatar is used.
