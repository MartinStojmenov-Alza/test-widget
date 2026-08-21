# test-widget

GitHub Pages test site for Box2Box widget integration.

Live at: **https://martinstojmenov-alza.github.io/test-widget/**

## What this tests

This static page exercises the Box2Box box-picker widget (`box2boxwidget`) by embedding the
loader script from a widget host and opening the picker iframe. It also tests the public REST
API endpoints directly via `fetch`.

## Features

- **Environment switcher** — Dev / Test / Prod widget host selector
- **Widget embed** — loads `loader.js` and calls `Box2BoxPicker.open()` with configurable options
- **Result display** — pretty-prints the `selectedBox` payload and logs all `postMessage` results
- **Direct API test** — calls `GET /api/v1/public/boxes` and `GET /api/v1/public/boxes/search`

## Widget hosts

| Environment | URL |
|-------------|-----|
| Dev (internal) | `https://box2boxwidget-popr-alzaboxesconnectors.d-dc1-k8sdmz01.alz.lcl` |
| Test | `https://box2boxwidget-test.alza.cz` |
| Prod | `https://box2boxwidget.alza.cz` |

## How to use

1. Go to the [live page](https://martinstojmenov-alza.github.io/test-widget/)
2. Select a widget host (Test is the default)
3. Optionally configure countries, language, and partner API key
4. Click **Open Box2Box Picker**
5. Pick a box in the overlay — the result is logged and displayed
6. Scroll down to test the direct REST API endpoints

## CORS

The widget's `feature/cors-fix` branch adds wildcard CORS (`AllowAnyOrigin`). Until that merges
and deploys, cross-origin `fetch` calls from this GitHub Pages origin to the widget host may be
blocked by the browser. The iframe-based widget embed works regardless of CORS because iframes
load pages cross-origin by design (the loader uses `postMessage` for communication).
