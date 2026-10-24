# DELIRIO · Carta digital

Responsive Spanish restaurant menu based on the supplied seven-page DELIRIO PDF. Includes all menu entries and source prices, category navigation, accent-insensitive search, four pizza sizes, a pizza customizer with animated additions inspired by the supplied video, and a browser-local order summary.

## Run

```sh
npm start
```

Open http://localhost:4173. No dependency installation or build step is required. Serve `dist/` with any static web host.

## GitHub and Vercel

Push the project contents (`dist/`, `package.json`, `tests/`, `README.md`, `.gitignore`, and `vercel.json`) to a GitHub repository. The GitHub-ready export excludes the original working repository's Git metadata and `.openai/` hosting configuration; neither is needed on Vercel. Do not upload the parent workspace's reference video or temporary QA files.

In Vercel, create a new project, connect GitHub, and import that repository. Keep the root directory at the repository root, select **Other** as the framework preset, leave the build and install commands empty, and use **dist** as the output directory. `vercel.json` supplies those deployment settings. No environment variables or backend services are required. `npm start` is for local previews only, not a Vercel build command.

Future pushes to the connected production branch trigger deployments. Restaurant ordering is still a browser-local summary, not a kitchen or payment integration. Configure the audience and custom domain in Vercel as needed; the existing Sites deployment remains separate.

```sh
npm test
```

## Update the menu

Edit `dist/menu.js` for menu names, descriptions, prices, groups, and sizes. Prices are integer Colombian pesos. A pizza price array is ordered: slice, personal, medium, large. Branding and the original PDF are in `dist/assets/`.

`dist/pizza.js` contains original procedural SVG food illustrations and shared ingredient animations. `dist/pasta.js` adds illustrated plates for Carbonara, Boloñesa, Primavera, and Frutos del Mar, with distinct animated spaghetti strands and fettuccine ribbons. Pasta shape changes retain selected additions; the cart and copied summary include both. Images are illustrative, not photographs of actual dishes. Logo and mascots were extracted from the provided PDF. The web font has system fallbacks. Reduced-motion preferences are supported.

## Order behavior

The cart is saved to localStorage on the guest's device. All totals are derived from current menu data. Guests can copy their selection and call the number printed in the source menu. There is no server order submission, payment, kitchen integration, or availability backend. Two-flavor combinations and the beverage michelada supplement are noted for confirmation with the restaurant; they are not configurable in the cart.

The published Site starts private. Public restaurant/QR access requires a separate audience change.

## Budget-phone performance

`dist/performance.js` automatically selects lightweight mode on page load when available browser signals report 4 GB memory or less, 4 CPU cores or fewer, data saving, a slow connection, or reduced motion. There is no guest-facing switch or setup step. Legacy manual preferences are ignored, and the cart is preserved. Unavailable hardware signals are ignored. Reduced-motion preferences remain respected.

Lightweight mode uses fewer SVG elements, system fonts, immediate ingredient updates, and no blur or shadow filters. Full mode retains the ingredient animations and uses locally hosted optional fonts. Hidden mascot images download only when needed; closing a dish releases its preview elements. Search updates are debounced. All menu data, customization options, prices, and cart behavior remain available in both modes.

Browser checks on 2026-10-23 used Chromium CPU/network throttling, not physical phone hardware:

- At 6x CPU slowdown, 150 ms latency and 1.28 Mbps, observed startup resource transfer fell by approximately 100 KB; the tested pasta preview fell from 507 SVG elements to 202.
- At 8x CPU slowdown, 400 ms latency and 400 kbps, with a simulated 2 GB / 2-core device and a 320px viewport, DOM readiness was about 3.1 seconds in one run. The largest recorded interaction duration in that scripted flow was 72 ms. This is a lab observation, not a field INP score or device guarantee.
- Pizza/pasta additions, size/shape changes, cart totals, persistence, search, automatic mode selection (including stale manual preferences), reduced motion, and deferred non-pizza illustrations were exercised. The Node test suite contains eight passing tests.

Physical budget-Android and iPhone testing is still needed before public rollout. The app targets modern mobile browsers; throttling does not reproduce every GPU, browser, thermal, or memory constraint.
