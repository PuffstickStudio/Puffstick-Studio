# Project notes for agents

## What this is
`index.html` is the entire application: a single static page using React 18
(UMD), Babel Standalone (`type="text/babel"`), and Tailwind, all loaded from
public CDNs at runtime. There is no `package.json`, no build step, no backend,
and no test suite.

State (products, cart, customer info, logo, batch status) is persisted in the
browser only, via `localStorage` keys prefixed `puffstick_`. Uploaded images
are read client-side with `FileReader` and stored as data URLs — no server or
storage service is involved. Outgoing orders go through a `wa.me` link.

## Running it
```sh
docker compose -f docker-compose.base44.yml up -d --build
```
Serves the bind-mounted `index.html` on host port 3000 through nginx
(`.base44/nginx.conf`, `Cache-Control: no-store`). Edits to `index.html` appear
on refresh — there is no dev server or watcher, so reload the preview after a
change.

## Verifying it works
`curl -s http://localhost:3000/ | head` should return the page, and the
compose healthcheck (`wget` against `/index.html`) reports healthy. A blank
page in the browser means React/Babel failed to load or a Babel transform
error — check the browser console, since nothing is precompiled.
