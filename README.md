# tricky-arrows-legal

The Privacy Policy and Terms of Service for **Tricky Arrows**, an Android puzzle game
by Golden Garuda Studios. Published as a static site with GitHub Pages.

Google Play requires a reachable Privacy Policy URL on the store listing, and the app
itself links to both pages from its Settings screen. That makes this repo a
**production dependency of the shipped app**, not documentation.

## Live URLs

    https://ggaruda-studios.github.io/tricky-arrows-legal/
    https://ggaruda-studios.github.io/tricky-arrows-legal/privacy
    https://ggaruda-studios.github.io/tricky-arrows-legal/terms

## Publishing

1. Push these files to the default branch of `GGaruda-Studios/tricky-arrows-legal`.
2. **Settings → Pages → Source: Deploy from a branch → `main` / `/ (root)`.**
3. Wait for the first build, then open all three URLs above in a browser.

Step 3 is not optional. A Privacy Policy URL that 404s is worse than a missing one:
Play will reject the listing, and a released app would carry dead links in Settings.

## Do not rename these paths

The app hardcodes the base URL and appends `privacy` and `terms`:

    ui/main.gd
    const DOCS_URL := "https://ggaruda-studios.github.io/tricky-arrows-legal/"
    ... OS.shell_open(DOCS_URL + "privacy")
    ... OS.shell_open(DOCS_URL + "terms")

Every released build carries that string baked in. Renaming a folder, moving the repo,
renaming the repo, or changing the org name breaks the links **in apps already
installed on people's phones**, and no update can fix the copies that are not updated.
If a path ever has to change, leave a redirect at the old one.

## Why folders instead of privacy.html

The pages are `privacy/index.html` and `terms/index.html` rather than `privacy.html`
and `terms.html` so that the extensionless URLs the app requests are served by any
static host, with no reliance on a server's optional "strip the .html extension"
behaviour. A directory index is the one form that cannot 404.

## Why there are no webfonts, analytics or trackers

Deliberate, and it should stay that way.

- The privacy policy states the app has no third-party SDKs and sends nothing. A
  policy page that itself calls out to a third party — a CDN font, an analytics
  snippet — contradicts the document it is serving. Embedding Google Fonts without
  consent has been found to breach GDPR in at least one German ruling, precisely
  because it discloses the visitor's IP address.
- Every page is plain HTML plus one local stylesheet. Nothing is fetched from anywhere
  else, so reading the policy cannot be logged by anyone but GitHub.

`.nojekyll` is present so GitHub Pages serves these files as written instead of
running them through Jekyll.

## Layout

    index.html           landing page, links to both documents
    privacy/index.html   Privacy Policy      -> /privacy
    terms/index.html     Terms of Service    -> /terms
    style.css            shared styles, the app's own colour palette
    .nojekyll            serve as static files, do not run Jekyll

Colours are the game's "cream" and "slate" palettes, so the pages look like the app
in both light and dark mode. Links between pages are relative, never root-absolute,
because the site is served from a subpath (`/tricky-arrows-legal/`) rather than from
a domain root.

## Updating a document

1. Edit the HTML.
2. Change the `Last updated` date **and** the `datetime` attribute next to it.
3. If a change is material, say so in the app's Play release notes — both documents
   promise that.

If a future version of the app ever collects or transmits anything, the Privacy
Policy has to be updated **before** that version is released, and the Play Data
Safety answers have to change with it.
