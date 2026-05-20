# Pick & Scrape — Day 93 Web Scraping

A single-page web app where you **pick a website to scrape from**, hit a button, and get the
data back as a sortable table plus a downloadable **CSV**. Everything runs client-side — the page
fetches live HTML through a public CORS proxy and parses it in your browser, so it can be hosted
for free on GitHub Pages with no backend.

**Live site:** https://voyageplanetgames.github.io/web-scraping/

## Sources you can scrape

| Source | Data captured |
|---|---|
| Books to Scrape | Title, price, star rating, availability (3 pages) |
| Quotes to Scrape | Quote text, author, tags (3 pages) |
| Hacker News front page | Rank, title, points, comments, link (live) |
| Countries of the World | Name, capital, population, area (~250 rows) |

## How it works

1. You select a source card.
2. The app requests each page through a CORS-proxy chain (`allorigins` → `corsproxy.io` →
   `thingproxy`), falling back automatically if one is unavailable.
3. The returned HTML is parsed with the browser's native `DOMParser` and CSS selectors.
4. Results render in a table; **Download CSV** builds a UTF-8 CSV (with BOM, properly escaped)
   and saves it locally.

## Run locally

It's a static file — just open `index.html`, or serve it:

```bash
python3 -m http.server 8000   # then visit http://localhost:8000
```

## Add your photo

The header avatar loads `assets/profile.jpg`. Drop your own photo there (a built-in initial-based
avatar shows until you do). The ☕ button links to https://buymeacoffee.com/chenbuilds.

## Notes & limitations

- Public CORS proxies are convenience infrastructure and can rate-limit or go down; a retry
  usually fixes a transient failure. For production you'd run your own tiny proxy.
- Targets are public scraping sandboxes and open pages — scrape responsibly and respect
  `robots.txt` and rate limits on any real site.
