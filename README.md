[README.md](https://github.com/user-attachments/files/29542956/README.md)
# TikTok Uploader

A small, open source web app for reviewing and publishing pre-made videos to
TikTok using TikTok's official Login Kit and Content Posting API (Direct Post).

There is no shared server and no hosted version of this app. Everyone who
uses it runs their own copy on their own computer and connects their own
TikTok account. Nothing about your account, your videos, or your credentials
is ever sent anywhere except directly to TikTok's own API.

## What it does

1. You connect your own TikTok account through TikTok's official login screen.
2. The app shows you a video waiting in your `review/` folder, along with its
   caption.
3. Before posting, you manually:
   - choose who can see the post (nothing is pre-selected)
   - choose whether comments, duets, and stitches are allowed (all off by
     default)
   - confirm you consent to publish
4. Only once all three are set does the Publish button activate. The app then
   sends the video to TikTok using Direct Post.

## Setup

### 1. Register your own TikTok app

Go to [developers.tiktok.com](https://developers.tiktok.com/), create an app,
and add these products:
- **Login Kit**
- **Content Posting API** (with Direct Post enabled)

Add a Redirect URI matching where you'll run this app, for example:
```
http://localhost:5000/auth/callback
```

You'll be given a **Client Key** and **Client Secret** — keep the secret
private, never commit it to a public repo.

### 2. Install dependencies

```bash
pip install flask requests
```

### 3. Set your environment variables

```bash
# macOS/Linux
export TIKTOK_CLIENT_KEY="your_client_key"
export TIKTOK_CLIENT_SECRET="your_client_secret"

# Windows (PowerShell)
setx TIKTOK_CLIENT_KEY "your_client_key"
setx TIKTOK_CLIENT_SECRET "your_client_secret"
```

If you're running on a different host/port than `localhost:5000`, also set:
```bash
export TIKTOK_REDIRECT_URI="http://your-host:port/auth/callback"
```
(must exactly match a Redirect URI registered on your TikTok app)

### 4. Add a video to review

Create a `review/` folder next to `app.py`, and put an `.mp4` file in it.
Optionally add a matching `.json` file with the same name containing:
```json
{
  "title": "Your video title",
  "social_caption": "Caption to use when posting"
}
```

### 5. Run it

```bash
python app.py
```

Open `http://localhost:5000`, click **Connect TikTok Account**, log in, and
you'll land on the review screen.

## Notes

- This app only ever talks to TikTok's own API directly — there's no
  middleman server.
- Access tokens are saved locally to `tiktok_token.json` and are never
  uploaded anywhere.
- TikTok requires Direct Post integrations to go through an audit before
  posts can be public; until then, posts are restricted to private
  (`SELF_ONLY`) viewing. See TikTok's
  [Content Sharing Guidelines](https://developers.tiktok.com/doc/content-sharing-guidelines)
  for details.

## License

MIT — see `LICENSE`.
