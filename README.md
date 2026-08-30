# pcarbuddy-site

The index page at **https://tools.pcarbuddy.com** — one list of every car
project, what it does, and where it runs.

It exists because there are fifteen of them across two hosts, three of the URLs
are `*-production.up.railway.app` strings nobody can recall, and the answer to
"which one had the Monroney parsing?" was previously `ls ~/Projects`.

## Pages

| Path | File | Used for |
|---|---|---|
| `/` | `site/index.html` | The whole site. There is only one page. |

## It is deliberately not indexed

`index.html` carries `<meta name="robots" content="noindex, nofollow">`. The
page names internal hostnames and repo paths, which is fine for anyone holding
the link and pointless to hand to a crawler. Delete the tag to list it.

Nothing here is a secret — every URL on the page is already reachable, and the
gated ones (`porsche-tracker`, `monroneybuddy`) answer 403/401 to a stranger
either way. `noindex` is tidiness, not a security control.

## Design

`site/style.css` is the shared design system from `odhllc-site`,
`garage-buddy-site` and `workout-buddy-site` — everything from "Tokens" to
"Footer" is byte-identical to those three. Only two things differ:

- the brand block at the top (`--accent: #c4342a`, redline red)
- the **Index** block, this site's equivalent of Garage Buddy's odometer hero

Copied rather than shared, for the same reason the other three copy it: four
self-contained repos beat a package for 300 lines of CSS.

## Adding a project

Add an `<li>` to the right `<ul class="index">` — Live, iPhone, or Not
deployed — with a `.name`, an optional `.tag`, a `<p>`, and a `.where` line
carrying the hostname and the repo path. No CSS changes needed.

Move a project between sections when it ships; the sections are the only status
tracking here, which is the right amount for a page that exists to answer
"where is that thing."

## Deploy

Push to `main`. GitHub Actions publishes `site/` to GitHub Pages
(`.github/workflows/pages.yml`), same as the other site repos.

DNS lives at **Cloudflare** (`pcarbuddy.com`), not DreamHost:

```
CNAME   tools   shuffman.github.io   DNS only (grey cloud)
```

The grey cloud matters — proxying through Cloudflare in front of GitHub Pages
breaks Pages' own certificate issuance for the custom domain.
