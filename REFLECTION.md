# Day 93 — Reflection

**How I approached it.** The brief was "scrape a site and build a CSV," but I pushed it into a
small product: a page where the *user* picks which site to scrape. That framing forced a real
design decision up front — where does the scraping actually run? A browser can't fetch arbitrary
third-party pages because of CORS, so the honest options were (a) a backend, (b) ship pre-scraped
snapshots, or (c) route browser requests through a CORS proxy. I chose the proxy route because it
keeps the whole thing static, free to host on GitHub Pages, and still genuinely *live*.

I structured the code around a single `SOURCES` config object — each source declares its URL(s),
its CSV columns, and a `parse(document)` function. Adding a fifth site later is just one more
entry, no plumbing changes. Scraping, table rendering, and CSV export are all generic and read
from that config.

**What was easy.** The parsing itself. Once you've done a couple of `querySelectorAll` passes the
pattern is muscle memory: select the repeating container, pull fields by class, trim whitespace.
The CSV builder was also quick — the only real gotcha is escaping commas/quotes and adding a UTF-8
BOM so Excel doesn't mangle the `£` and `km²` characters.

**What was hard.** Two things. First, CORS — accepting that the browser genuinely cannot just
`fetch()` another domain, and that a proxy is a real dependency I don't control. I handled that
with a fallback chain of three proxies and clear error messaging instead of pretending it'll
always work. Second, Hacker News markup: the score and comment count live in the *sibling* row of
the title, not inside it, so the parser has to hop to `nextElementSibling`. That's the kind of
thing you only find by actually reading the DOM.

**Biggest learning.** Scraping is the easy 20%; the resilience around it is the other 80% —
proxy fallbacks, "0 rows parsed" detection, graceful failures, and not trusting that a page's
structure stays put. A scraper that works once in a notebook is very different from one a stranger
can click on a website.

**What I'd do differently next time.** I'd stand up a tiny serverless proxy (a single Cloudflare
Worker) instead of relying on public ones, so the live demo isn't at the mercy of someone else's
rate limit. I'd also add a caching layer so re-scraping the same source within a few minutes is
instant, and pagination controls so the user chooses how deep to go rather than me hard-coding
three pages. Finally I'd write the parsers against saved HTML fixtures from the start (I did this
for verification at the end) so structure changes get caught by a test rather than by a user
seeing an empty table.
