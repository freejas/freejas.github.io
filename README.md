# Jason D. Freeman — Personal Academic Website

Plain static site (HTML + CSS, one tiny JS file for the mobile nav toggle).
No build step, no framework, no webfonts. Deployed to **GitHub Pages** at
**jasondfreeman.com**.

Content source of truth: Jason's CV dated 2026-11-16
(`Jason Freeman CV 11.16.2026.pdf`). All facts, dates, publications, and talks
on the site come from that CV or from URLs verified on the previous Google Site.
No citation counts / h-index are shown (they go stale).

## Structure

```
.
├── index.html          Home: hero, bio, research interests, news, positions, education
├── research.html       Research areas + selected talks/posters 2023–2026
├── publications.html   Peer-reviewed, under review, in preparation
├── teaching.html       Instructor of record + teaching assistant roles
├── contact.html        Email, affiliation, verified profile links
├── 404.html            Custom not-found page (GitHub Pages picks this up)
├── styles.css          Single shared stylesheet (mobile-first, responsive)
├── script.js           Mobile nav toggle only
├── CNAME               Custom domain: jasondfreeman.com
├── .nojekyll           Tells GitHub Pages to serve files as-is (no Jekyll)
└── assets/
    ├── portrait.png        Portrait photo (480×480, from the old Google Site)
    └── Jason-Freeman-CV.pdf  CV, linked from the nav and home page
```

## Deploy to GitHub Pages

1. Create a repo on GitHub (e.g. `jasondfreeman.github.io` for a user site,
   or any repo name for a project site) and push this directory to it.
2. In the repo: **Settings → Pages → Build and deployment → Deploy from a branch**,
   branch `main`, folder `/ (root)`.
3. The `CNAME` file sets the custom domain automatically once Pages is enabled.
4. DNS (at Squarespace, where jasondfreeman.com is registered) — point the
   apex domain at GitHub Pages with A records for GitHub's documented Pages IPs
   (`185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`),
   and add a `www` CNAME to `<username>.github.io`. Verify the current IPs in
   GitHub's docs at deploy time, as they can change.
5. In the Pages settings, enable **Enforce HTTPS** once the certificate is issued
   (GitHub provisions it automatically for the custom domain; may take up to ~24h).

## Updating content

- Edit the HTML files directly — every page shares the same header/nav/footer
  markup, so a nav change must be made in all six `.html` files.
- To update the CV, replace `assets/Jason-Freeman-CV.pdf` (keep the filename).
- To update the portrait, replace `assets/portrait.png` (square, ~480px).
- After editing, re-check that all internal links resolve (see below).

## Local check

```sh
cd jasondfreeman-site
python3 -m http.server 8000
# then open http://localhost:8000 in a browser
```

Link check (all internal `href`/`src` targets must exist):

```sh
python3 - <<'EOF'
import re, pathlib
root = pathlib.Path('.')
ok = True
for f in root.glob('*.html'):
    text = f.read_text()
    for target in re.findall(r'(?:href|src)="([^"#]+)(?:#[^"]*)?"', text):
        if target.startswith(('http', 'mailto:')):
            continue
        if not (root / target).exists():
            print(f'BROKEN in {f}: {target}')
            ok = False
print('All internal links OK' if ok else 'Broken links found')
EOF
```
