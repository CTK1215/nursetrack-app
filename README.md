# nursetrack.app

Marketing site and legal pages for NurseTrack, a mobile app for home-health and 1099 nurses that drafts visit notes, logs mileage, and orders the day's visits into a route. Plain HTML and CSS, no build step, no dependencies.

Live: [https://nursetrack.app](https://nursetrack.app)

## What's on the site

| Path | File | What it is |
|---|---|---|
| `/` | `index.html` | Landing page: hero with a phone mockup, problem stats, four feature sections, a day timeline, a privacy section, pricing, and a waitlist form |
| `/get` | `get.html` | Beta install chooser with iPhone and Android buttons plus QR codes. Marked `noindex` |
| `/ios`, `/android` | (redirects) | Send the visitor to the TestFlight build or the Android build |
| `/privacy` | `privacy.html` | Privacy Policy, including the SMS appointment-reminder notice for patients |
| `/terms` | `terms.html` | Terms of Service, including the SMS reminder terms |

## Stack

- Hand-written HTML and CSS. One small inline script on the landing page.
- System font stack, no external requests, no analytics.
- No `package.json`, no bundler, nothing to install.

## Project structure

```
index.html      landing page
get.html        beta install chooser
privacy.html    Privacy Policy
terms.html      Terms of Service
styles.css      the whole theme: tokens, layout, components, legal-page styles
assets/         wordmark (light and dark), app icon, QR codes
vercel.json     clean URLs and the /get, /ios, /android redirects
```

## Running locally

Nothing to install. Open `index.html` in a browser, or serve the folder with a static server that supports clean URLs so `/privacy` and `/terms` resolve the way they do on Vercel:

```
npx serve .
```

No environment variables.

## Deployment

Vercel deploys from the `main` branch as a static site. `vercel.json` sets `cleanUrls: true`, which is why `privacy.html` is served at `/privacy` and `get.html` at `/get`, and it defines the redirects described below.

## Notes on the implementation

- **Device-aware install links.** `/get` is redirected by user-agent in `vercel.json`: Android devices go to `/android`, iPhones and iPads go to `/ios`, and those two paths redirect on to the TestFlight join page and the Android build. Desktops and unknown devices fall through to `get.html`, which shows both buttons and QR codes to scan with a phone.
- **Theme mirrors the app.** `styles.css` defines the palette as CSS custom properties with a light default and a full dark palette under `prefers-color-scheme: dark`. The wordmark swaps between light and dark PNGs with a `<picture>` element.
- **Waitlist without a backend.** The landing-page form validates the email, remembers it in `localStorage`, and opens a pre-filled `mailto:` draft. The code comments call this out as the interim path until an endpoint exists.
- **Motion respects preferences.** Reveal-on-scroll uses `IntersectionObserver` and is skipped entirely when `prefers-reduced-motion` is set.
