# NFC Toolkit — marketing & SEO site

Static website for the iPhone app **NFC Toolkit** ([App Store](https://apps.apple.com/app/id6818562159)).
Live at **https://ggfresard.github.io/nfc-toolkit-site/** (GitHub Pages).

Plain HTML + one CSS file. No framework, no build step, no JavaScript, no external fonts/CSS, no cookies, analytics or trackers.

## Structure

```
index.html                 Landing page (features, Free vs Pro, screenshots, guides, FAQ + FAQPage JSON-LD)
guides/*.html              Five how-to guides (Article + HowTo + BreadcrumbList JSON-LD)
support.html               Troubleshooting FAQ (FAQPage JSON-LD) + contact
privacy.html               Privacy policy (copied verbatim from the app repo's docs/privacy-policy.md)
404.html                   Not-found page (uses absolute /nfc-toolkit-site/ paths because Pages serves it at any depth)
sitemap.xml, robots.txt    SEO files (absolute URLs with the base path)
assets/style.css           The only stylesheet (~5 KB; light + dark mode via prefers-color-scheme)
assets/icon.svg            Favicon
assets/img/screen-0N.*     App Store screenshots resized to 660 px wide, WebP + JPEG fallback
.nojekyll                  Tells GitHub Pages to serve files as-is (no Jekyll processing)
```

## Editing

- **Text:** edit the HTML files directly. Each page is self-contained.
- **Header / footer / `<head>`:** these are repeated in every page (there is no build step). If you change the nav, footer or a shared meta tag, change it in all 9 HTML files (a project-wide find & replace works well).
- **Links:** keep internal links **relative** (`support.html`, `../privacy.html`, `guides/wifi-nfc-tag.html`). The site lives under `/nfc-toolkit-site/`, so root-absolute links like `/support.html` would break. Only `404.html` uses `/nfc-toolkit-site/...` paths, on purpose.
- **New page:** copy an existing page, then update `<title>`, meta description, canonical, `og:url`, `og:title`/`og:description`, the JSON-LD block, and add the URL to `sitemap.xml`. Keep `<meta name="apple-itunes-app" content="app-id=6818562159">` (Safari Smart App Banner).
- **Screenshots:** regenerate from `docs/store-screenshots/en/6.9/` in the app repo, e.g.
  ```
  python3 -c "from PIL import Image; im=Image.open('01.png').convert('RGB'); r=im.resize((660, round(im.height*660/im.width)), Image.LANCZOS); r.save('screen-01.webp', quality=76, method=6); r.save('screen-01.jpg', quality=78, optimize=True, progressive=True)"
  ```
- **Prices / claims:** keep them in sync with the App Store listing. Don't add ratings, reviews or download numbers unless they are real.

## Preview locally

The site expects to live under `/nfc-toolkit-site/`, so serve the parent folder:

```
cd .. && python3 -m http.server 8000
# open http://localhost:8000/nfc-toolkit-site/
```

## Deploying (GitHub Pages)

1. Push this folder to the public repo `ggfresard/nfc-toolkit-site` (default branch `main`).
2. GitHub → repo **Settings → Pages → Build and deployment → Source: Deploy from a branch**, branch `main`, folder `/ (root)`.
3. The site appears at `https://ggfresard.github.io/nfc-toolkit-site/` after a minute or two. Every push to `main` redeploys.

After the first deploy:
- Add the property `https://ggfresard.github.io/nfc-toolkit-site/` in **Google Search Console** (URL-prefix property, verify with the HTML-file method by dropping the file in this repo) and submit `sitemap.xml`. Same in Bing Webmaster Tools if wanted.
- Note: crawlers only read `robots.txt` at the **host root** (`https://ggfresard.github.io/robots.txt`), so this repo's `robots.txt` is ignored on a project site. It's kept for when the site moves to its own domain. Submit the sitemap manually instead.
- Set the App Store Connect **Support URL** to `https://ggfresard.github.io/nfc-toolkit-site/support.html` and (optionally) the **Marketing URL** to the home page. The **Privacy Policy URL** can point to `.../privacy.html`.

## Custom domain (optional, later)

Add a `CNAME` file with the domain, configure DNS per GitHub's docs, then replace every `https://ggfresard.github.io/nfc-toolkit-site/` in canonicals, `og:` tags, JSON-LD and `sitemap.xml`, and the `/nfc-toolkit-site/` paths in `404.html`.

## App Store badge

The download buttons are styled text links, not Apple's badge artwork. If you want Apple's official "Download on the App Store" badge, download it yourself from Apple's marketing resources (https://developer.apple.com/app-store/marketing/guidelines/ → badges / https://tools.applemarketingtools.com), save the SVG into `assets/`, and follow Apple's badge usage guidelines. Don't hotlink it.
