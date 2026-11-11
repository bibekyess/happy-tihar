# Happy Tihar 🎁

A small static site for sending a **Bhai Tika gift** over KakaoTalk, hosted on GitHub Pages.

The recipient opens the gift page and sees a short prank video: the gift box holds a spider 🕷️ and ends with "Balla paryo makuri jalaima", a famous Nepali folk song line meaning "someone has finally fallen into a trap".

## Two ways to send the gift

### Option 1: Paste the link (no setup)

Paste https://bibekyess.github.io/happy-tihar/ into any KakaoTalk chat. KakaoTalk shows a link preview with an image, title, and description, and tapping it opens the gift page.

This works right away, but the preview layout is fixed by KakaoTalk and can't have buttons.

### Option 2: Send a custom card (needs a Kakao app)

Open the send page, https://bibekyess.github.io/happy-tihar/send.html, on your phone and tap **카카오톡으로 보내기**. KakaoTalk sends a richer card with a yellow banner, a **선물 받기** button, and your app's name and icon at the bottom. Tapping the card opens the gift page.

1. Make sure the KakaoTalk app is installed and logged in.
2. Tap **카카오톡으로 보내기**, pick a friend or chat, and tap **확인**.
3. Repeat for each recipient.

Send a test to **나와의 채팅** (chat with yourself) first. Keep the send page link to yourself and share only the card.

This option needs the one-time setup below.

## Kakao app setup (Option 2 only)

1. At https://developers.kakao.com/console/app, click **Create App**.
2. Set the **app name** and **app icon**. They appear at the bottom of every card. I used "Vaiko", `images/original/vaikopay-logo-800x240.png`, and the category "Entertainment", since this was a one-day project.
3. Go to **App → Platform Key → Add JavaScript Key** and set the JavaScript SDK domain to `https://bibekyess.github.io`.
4. Copy the JavaScript key into `Kakao.init('...')` in `send.html`.

The JavaScript key is meant to be public, because it only works on the registered domain. **Never** put the REST API key or Admin key in a page.

## Files

```
index.html        Gift page. Plays the video muted on loop, with a 🔊 Tap for sound button.
                  Its og: tags build the Option 1 link preview.
send.html         Send page for Option 2: card preview and share button.
favicon.svg       Browser tab icon.
videos/
  original/       Source video exports (not committed).
  processed/      bhai-tika.mp4, the compressed gift video.
images/
  original/       kakao-preview.jpg (mock-up the card banner came from),
                  vaikopay-logo-800x240.png (Kakao app icon).
  processed/      kakao-card.jpg (Option 2 card banner),
                  nepali-gift-preview.jpg (Option 1 link preview image),
                  bhai-tika-poster.jpg (shown while the video loads).
```

## Running and deploying

There's no build step. To preview locally:

```bash
python -m http.server 8000   # open http://localhost:8000/
```

The share button only works locally if you also register `http://localhost:8000` as a JavaScript SDK domain. To deploy, push to `main`. GitHub Pages updates in 1–2 minutes.

## Changing things

| To change… | Do this |
|---|---|
| Link preview text or image (Option 1) | Edit the `og:` and `twitter:` tags in `index.html`. |
| Card title, description, or button (Option 2) | Edit `title`, `description`, or `buttons` in the `<script>` of `send.html` (at most 2 buttons). |
| Card banner image (Option 2) | Replace `images/processed/kakao-card.jpg` and update `imageWidth`/`imageHeight` in `send.html`. |
| Card footer name or icon (Option 2) | Change them in the Kakao Developers console. No code change. |
| Where the card opens | Change `const url` in `send.html`. |
| Gift video | Replace `videos/processed/bhai-tika.mp4` and its poster. Keep it a few MB so it loads fast on mobile. |

KakaoTalk caches images and previews by URL. After replacing an image, give it a new filename or bump the `?v=N` at the end of its URL.

## Troubleshooting

| Problem | Fix |
|---|---|
| Domain error, or nothing happens on tap | Register `https://bibekyess.github.io` exactly (no path or trailing slash) as the JavaScript SDK domain. |
| KakaoTalk doesn't open | Check the app is installed and logged in, or open `send.html` from inside KakaoTalk (send yourself the link). |
| Video shows a still image | Autoplay was blocked (e.g. iPhone Low Power Mode). Tap the video. |
| Video has no sound | Phones only autoplay muted video. Tap **🔊 Tap for sound**. |
| Old image or preview still shows | KakaoTalk cache. Rename the file or bump `?v=N`. For link previews you can also clear it at https://developers.kakao.com/tool/debugger/sharing. |
| `Kakao is not defined` | The SDK script failed to load (network, ad blocker, or wrong version URL). |

Keep the card clearly a Tihar gift. Kakao can suspend apps whose cards imitate Kakao services such as KakaoPay transfers.
