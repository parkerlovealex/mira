# MIRA — Dating discovery demo

A fresh static dating-app interface inspired by the latest supplied references. It uses a dark olive/black base, coral red, lime and white, with a Gilroy-like geometric font fallback (Manrope).

## Files

- `index.html` — complete responsive application (HTML, CSS and vanilla JavaScript)
- `link.txt` — global redirect destination shared by every visitor
- `vercel.json` — disables caching for `link.txt` so new destinations are picked up promptly
- `README.md` — deployment and redirect instructions

## Global redirect (important)

`link.txt` is the single source of truth for the global destination.

1. Edit `link.txt` in the GitHub repository root.
2. Put exactly one full URL on its own line, such as `https://example.com/landing`.
3. Commit the change to the production branch (usually `main`).
4. Vercel automatically deploys the commit when GitHub integration is connected.

The app fetches `./link.txt` on page load and rechecks it every 60 seconds with cache-busting and `cache: no-store`. `vercel.json` also sets no-cache response headers for this file.

If `link.txt` is blank or invalid, external CTA buttons open the Global Redirect panel instead of sending visitors to a fake destination.

### Redirect parameters

All external CTAs call the same `redirectToGlobal()` function. It preserves existing destination query parameters and adds:

- `source=mira`
- `action=open|connect|chat|connections`
- `profile_id=<profile-id>` (for profile actions)
- `profile_name=<profile-name>`
- `profile_city=<profile-city>`

Swipe pass/like/super-like remain local discovery actions. Match "Say hello", profile "Connect", chat external-link, "Open destination", and "Explore connections" use the global redirect.

## Deploy on Vercel

1. Create a GitHub repository.
2. Upload `index.html`, `link.txt`, `vercel.json`, and `README.md` to the repository root.
3. Set the destination in `link.txt` and commit.
4. In Vercel, choose **Add New → Project** and import the GitHub repository.
5. Select **Other** as the framework preset.
6. Leave Build Command empty. Use `.` as the Output Directory if Vercel asks.
7. Deploy.

After initial connection, commits to the production branch trigger automatic deployments. Editing `link.txt` is all you need to change the shared destination.

## Demo limitations

- Sample profiles, matches, likes and messages are stored in the current browser using localStorage. They are not shared between visitors.
- Calls are not implemented; connect an authenticated backend and real-time/calling provider for a live service.
- Profile images, Manrope font, Tailwind CDN and Font Awesome CDN require an internet connection.
- Manrope is used as a close geometric alternative because Gilroy is a commercial font and is not bundled.


## Approximate IP-based location (no GPS permission)

MIRA attempts a coarse location lookup automatically when the page loads. It does **not** call `navigator.geolocation`, request GPS access, or show a browser location-permission prompt.

- Primary provider: `ipwho.is`
- Fallback provider: `ipapi.co` (used only if the primary lookup fails)
- The browser sends the visitor's public IP to the provider so it can return an approximate city/region and coordinates.
- Visitors can select a city manually or choose `Auto · IP` to retry.
- If both providers fail, the app keeps discovery available and asks the visitor to choose a city.
- IP geolocation can be wrong due to VPNs, mobile-carrier routing, proxies or ISP registration data.
- Demo profile distances use approximate city-centre coordinates, not exact member coordinates. Do not present these distances as precise real-world distances.

If you want to avoid sending IP addresses to third-party geolocation providers, disable `detectLocationByIP()` and keep manual city selection enabled.
