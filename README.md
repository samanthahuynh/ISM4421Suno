# SongForge — AI Music Generator

A one-page web app for generating songs with the [Suno API](https://docs.sunoapi.org).
It's a single static HTML file — no build step, no backend, no serverless functions.

## Features

- **Bring your own API key** — each user pastes their Suno API key (get one at
  [sunoapi.org/api-key](https://sunoapi.org/api-key)). It's saved only in that browser's
  localStorage and sent only to `api.sunoapi.org`.
- **Simple mode** — describe a song; lyrics are written for you.
- **Custom mode** — title, style tags, your own lyrics, excluded styles, vocal gender,
  length, style adherence, weirdness and variety.
- **AI lyric writer** — generate lyric options from an idea and drop one into the song.
- **Instrumental toggle** and model picker (V6, V6 Wild, V6 Mini).
- **Live progress** — streaming preview while the full MP3 renders, then download.
- **Library** — every generation (2 variations each) is kept in your browser, with lyrics,
  one-click "Reuse", and pending jobs that resume after a page reload.
- **Credit balance** shown in the header.

## Deploy to Netlify

The repo includes `netlify.toml`, which publishes the `public/` folder.

1. In Netlify: **Add new site → Import an existing project →** pick this repo.
2. Leave the build command empty; publish directory is `public` (read from `netlify.toml`).
3. Deploy. Open the site and paste your API key when prompted.

No environment variables are needed.

## Run locally

```sh
cd public && python3 -m http.server 8000
# open http://localhost:8000
```

## Notes

- Suno keeps generated files for 14 days — download the MP3s you want to keep.
- The API requires a `callBackUrl`; the app sends the site's own URL and polls
  `GET /api/v1/generate/record-info` for results instead of relying on callbacks.
