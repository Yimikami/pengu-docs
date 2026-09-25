<br />

<p align="center">
  <img src="./public/icons/4274.jpg" width="128" height="128" />
  <h1 align="center">Pengu Docs</h1>
  <p align="center">Pengu Loader documentation with a redesigned homepage, built with VitePress.</p>
  <p align="center">👉 <a target="_blank" href="https://pengu.lol/">https://pengu.lol</a></p>
</p>

<br />
<br />

## How to run?

> **Node.js** version 20 or higher and **pnpm** version 9 or higher are required.

Clone the repo:

```
git clone --branch codex/landing-redesign https://github.com/Yimikami/pengu-docs
```

Install dependencies and start development.

```
pnpm i
pnpm dev
```

Build and preview the production.

```
pnpm build
pnpm preview
```

## Homepage assets

The homepage lives in `.vitepress/components/Home.vue` and `docs/index.md`.
Styles are scoped to the homepage; documentation uses the VitePress theme.

- `community-theme.webp`: [Pengu Loader's community showcase](https://github.com/PenguLoader#showcases).
- `indie-theme.webp`: [Indie theme by nomi-san](https://github.com/nomi-san/indie-theme).
- `balance-viewer.webp`: [Balance Buff Viewer by nomi-san](https://github.com/nomi-san/balance-buff-viewer).
- `profile-detail.webp`: an illustrative Pengu profile detail, not a community theme screenshot.

Community screenshots are examples, not a guarantee of current-patch compatibility.
Window-effect images come from the existing documentation. Barlow is
self-hosted with its OFL license file in `public/fonts/`.

## Vercel demo

`vercel.json` builds this fork with pnpm 9 and serves `.vitepress/dist` with clean URLs.
Link the repository to a separate Vercel project before deploying. Demo pages send a
`noindex` header, and Google Analytics only runs on the official Pengu domains.
The existing Cloudflare workflow still handles the upstream site's deployments.

## Help us translate the docs

First, you need to create a new folder in the **docs** folder. For example, `vi`
for Vietnamese.

```
docs/
  |__guide/           |
  |__runtime-api/     | -> english docs
  |__index.md         |
  ...
  |__vi/              | -> vietnamese docs
```

Next, copy the doc files and folders from the root of the docs folder (except
the language folders) and then paste them into your language folder.

```
docs/
  |__vi/
    |__guide/
    |__runtime-api/
    |__index.md
```

Finally, add your language to the .vitepress/config.ts (see the
[i18n](https://vitepress.dev/guide/i18n#internationalization) section on
VitePress docs to learn more).

```ts
export default defineConfig({
  ...
  locales: {
    root: {   // root english
      label: 'English',
      lang: 'en',
    },
    'vi': {   // added vietnamese
      label: 'Tiếng Việt',
      lang: 'vi',
      link: '/vi/',
    }
  }
  ...
})
```

When you have finished editing the translation, push your changes to your forked
repo, and then open a new Pull Request.
