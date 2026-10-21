# garyzaimont.com

Static portfolio site for artist Gary Zaimont. Originally a ~2010 PHP site built by factor1; converted to plain static HTML in October 2026 for hosting on Netlify.

## Structure

```
public/            <- the site (Netlify publish directory)
  index.html       home
  catalog/ bio/ statement/ studio/
  dancers/ animals/ people/ abstracts/ videos/   section menus
  works/<name>/    portfolio sliders and video pages
  media/           MP4 video (converted from the old Flash .flv files)
  css/ js/ images/ works/images/
  404.html  robots.txt  sitemap.xml
netlify.toml       301 redirects from old .php URLs, headers
tools/convert-from-php.py   one-time script used to generate public/ from the PHP source (reference only)
```

There is **no build step**. Edit the HTML in `public/` directly. The header/footer markup is duplicated in each page (it was a PHP include); if you change the nav or footer, change it in every `index.html`.

## Local preview

```
cd public && python3 -m http.server 8000
```

## Deploy

Connect this repo in Netlify (no build command, publish directory `public`, already set in `netlify.toml`). Set `www.garyzaimont.com` as the primary domain to match the canonical tags.

## What changed from the PHP version

- PHP includes flattened into static HTML; `<base href="http://...">` removed (it caused mixed-content CSS blocking under https). Asset and page paths are relative so pages style correctly from any host or opened from disk; `404.html` stays root-relative because Netlify serves it at arbitrary URLs.
- Clean URLs as directories (`/catalog/`). Old `*.php` URLs 301 to the new paths via `netlify.toml`.
- Flash video player (SWF + FLV) replaced with HTML5 `<video>` and H.264 MP4s.
- jQuery served locally (1.12.4) instead of the unpinned `jquery/1/` Google CDN over http.
- StatCounter moved to https. Legacy Universal Analytics (`ga.js`, UA-147568-36) removed; it stopped collecting data in 2023.
- Empty `<title>` tags filled in; meta keywords block removed; sitemap.xml and robots.txt added.
- Broken links on the old site fixed (`works/abstracts`, `works/videos`).

## Security note

The old host had been compromised: `seobot.php` plus a rewritten `.htaccess` served spam content from a remote server to Googlebot only (SEO cloaking). None of that code is in this repo.
