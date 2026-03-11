# MangaBaka URL Finder

Chrome extension for finding matching aggregator URLs for `mangabaka.org/<id>`

**Not affiliated with MangaBaka dev team**

## Features

- Reads MangaBaka metadata from active tab and finds URLs from chosen manga aggregator sites
- Caches and saves URL information locally
- Shows latest chapter info where supported
- Optionally save URL as the MangaBaka `Read Link` field for series already in your library

Notes:
- Avoids re-searching and minimizes background query count
- Lets you mark a provider result as incorrect and search again while excluding previous bad URLs
- Lets you retry provider searches with alternate MangaBaka title

## Providers

Enabled by default:
- `Atsumaru`
- `MangaDex`

Available in options and disabled by default:
- `MangaFire`
- `WeebCentral`
- `E-Hentai`
- `ExHentai`

Unavailable currently:
- `Comix`

Notes:
- `MangaFire` and `WeebCentral` can resolve URLs using a websearch, but chapter metadata is blocked behind site protection (`Cloudflare Error`)
- `Comix` is intentionally disabled in the options UI and is not searched
- `ExHentai` uses the same search logic as `E-Hentai`, but it only works when the browser already has the required authenticated cookies

-------------------------------------------------

## Framework

This project uses:
- Chrome Extension Manifest V3
- Plain TypeScript
- Static HTML and CSS
- No frontend framework
- No bundler

The compiled extension scripts are written to `dist/` by `tsc`.

## Project Layout

```text
manifest.json        Chrome extension manifest
popup.html           Popup markup
popup.css            Popup styles
src/popup.ts         Popup behavior, provider search, cache, MangaBaka save flow
src/background.ts    Toolbar icon state handling
assets/              Extension icons and provider favicons
dist/                Generated JavaScript output from TypeScript
```

## Scripts

```bash
npm run clean
npm run build
npm run watch
npm run package:store
```

What they do:
- `npm run clean` removes `dist/`
- `npm run build` cleans and recompiles the TypeScript sources
- `npm run watch` runs TypeScript in watch mode during development
- `npm run package:store` rebuilds the extension and prepares a runtime-only upload folder at `webstore-package/`

## Development

### Install dependencies

```bash
npm install
```

### Build

```bash
npm run build
```

### Load in Chrome

1. Open `chrome://extensions`
2. Enable `Developer mode`
3. Click `Load unpacked`
4. Select the project root:

```text
C:\Users\Nick\WebstormProjects\MangaURLExtension
```

### Reload after changes

1. Run `npm run build`
2. Reload the unpacked extension in Chrome

If Chrome keeps showing stale toolbar icons, remove the unpacked extension once and add it again.

## Chrome Web Store Upload

Prepare the upload bundle:

```bash
npm run package:store
```

That creates:

```text
webstore-package/
  manifest.json
  popup.html
  popup.css
  dist/
  assets/
```

Zip the contents of `webstore-package/` for upload.

Do not upload development-only files such as:

- `src/`
- `node_modules/`
- `.idea/`
- `package.json`
- `package-lock.json`
- `tsconfig.json`
- `Readme.md`

## MangaBaka Read Link Save Behavior

After copying a provider URL, the copy button turns into a blue save button. Pressing it will:

- verify the current series is on MangaBaka
- verify the series is in your MangaBaka library
- open or reuse the personal library editor for that series
- replace the existing `Read Link` value with the copied provider URL

If the series is not in your library, the extension reports that you must add it first.
