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
- **API authentication** — masked `X-Api-Key` input for both direct REST calls

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
6. Scroll down to the direct REST API tests, enter the selected environment's `X-Api-Key`, and select an endpoint

## Direct API authentication (`X-Api-Key`)

The field in section 5 supplies the `X-Api-Key` header for both direct REST calls. It is separate
from the optional **Partner API Key**, which is sent as the `partnerApiKey` query parameter.
The widget loader and picker options do not receive `X-Api-Key` from this field.

Leading and trailing whitespace is trimmed. Leaving the field empty omits the header, allowing
missing-key tests (expected HTTP 401). An invalid key also returns HTTP 401.

The value is masked and kept only in the page's input, not saved to browser storage or included
in request URLs or diagnostic logs. It remains visible in browser developer tools and accessible
to scripts running on the page. Use a suitable test key; never commit a real key to this repository.
When switching environments, replace it with the key for the selected environment.

`X-Api-Key` triggers a CORS preflight; the API must allow this header. The existing `no-cors`
diagnostic probe deliberately sends no API key.

## CORS

The widget's `feature/cors-fix` branch adds wildcard CORS (`AllowAnyOrigin`). Until that merges
and deploys, cross-origin `fetch` calls from this GitHub Pages origin to the widget host may be
blocked by the browser. The iframe-based widget embed works regardless of CORS because iframes
load pages cross-origin by design (the loader uses `postMessage` for communication).
