# SongForge — AI Music Generator

A one-page web app for generating songs with the [Suno API](https://docs.sunoapi.org).
It's a single static HTML file — no build step, no backend, no serverless functions.
Accounts (email + password) are handled by Supabase Auth.

## Features

- **Email login** — sign up, log in, forgot/reset password and log out (Supabase Auth).
  The app is locked until you log in, and each account keeps its own API key and library.
- **User profiles** — display name, unique @username, bio, favorite genres and a profile
  photo. New users are guided through setting one up on first login; edit it anytime from
  the profile button in the header. Favorite genres show first in Custom mode's style chips.
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

### One-time Supabase setting (required for the emails' links)

Supabase's confirmation and password-reset emails link back to your site, so tell Supabase
your site's address:

1. Open the Supabase dashboard → **Authentication → URL Configuration**.
2. Set **Site URL** to your Netlify URL, e.g. `https://your-site.netlify.app`.
3. Under **Redirect URLs**, add `https://your-site.netlify.app/**`
   (and `http://localhost:8000/**` if you test locally).

The Supabase project URL and publishable key are already in `public/index.html`. They're
meant to be public; nothing secret is stored in the repo.

## Database (Supabase)

Already applied to the project — nothing to run. The SQL is kept in `supabase/migrations/`:

- `profiles` table, one row per user, created automatically at sign-up by a trigger.
  Row Level Security: each user can read and edit only their own profile.
- `avatars` storage bucket (public read, images only, 2 MB max). Users can upload only
  into their own folder (`avatars/<user id>/`).

## Run locally

```sh
cd public && python3 -m http.server 8000
# open http://localhost:8000
```

## Notes

- Supabase's built-in email service is rate-limited (a few emails per hour) and meant for
  testing. For real users, add your own SMTP provider under **Authentication → Emails → SMTP Settings**,
  or turn off **Confirm email** under **Authentication → Sign In / Providers → Email** so new
  accounts can log in immediately.
- `public/vendor/supabase.js` is supabase-js v2.117.2, bundled so the app doesn't depend on a CDN.

- Suno keeps generated files for 14 days — download the MP3s you want to keep.
- The API requires a `callBackUrl`; the app sends the site's own URL and polls
  `GET /api/v1/generate/record-info` for results instead of relying on callbacks.
