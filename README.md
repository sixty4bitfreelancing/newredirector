# Banner Ad Redirect + Download Page

## How It Works

When a user clicks your banner ad, they land on this page which does **two things simultaneously**:

1. **Downloads a file** (PDF, ZIP, etc.) automatically in the background
2. **Redirects to your main website** after ~1.5 seconds — no intermediate "thank you" page

## Setup

### 1. Edit `index.html`

Change these three variables near the top of the `<script>` block:

```js
const DOWNLOAD_FILE = "assets/your-file.pdf";   // Path to your downloadable file
const REDIRECT_URL  = "https://www.yourmainwebsite.com";  // Your actual site URL
const DELAY_MS      = 1500;                     // ms before redirect (1500–2000 recommended)
```

### 2. Place your file

Put the downloadable file inside the `assets/` folder and update `DOWNLOAD_FILE` to match.

### 3. Deploy

Upload both `index.html` and the `assets/` folder to any web server (shared hosting, S3, Cloudflare Pages, etc.). Use the URL of this page as your banner ad link.

## Important Notes

| Issue | Workaround |
|-------|-----------|
| **Browser blocks auto-download** | Some browsers (especially Safari) require a user gesture for downloads. If the download doesn't start automatically, add `onclick` to an `<a>` tag instead of creating it via JS. |
| **Cross-origin files** | The file must be served from the same origin or have proper CORS headers if using `fetch`. Using a direct link (`<a href>`) avoids this. |
| **Mobile browsers** | iOS Safari may show a "Download Complete" banner but won't auto-open PDFs. This is expected behavior. |

## Alternative: Click-to-Start Version

If your browser blocks the automatic download, use this variant instead of the inline script in `index.html`:

```html
<a href="assets/your-file.pdf" download onclick="setTimeout(function(){window.location.replace('https://www.yourmainwebsite.com');}, 1500); return true;">
  <!-- Banner ad image goes here -->
</a>
```

This way the download is triggered by the actual click event (guaranteed user gesture), and the redirect fires after the delay.

## Testing Checklist

- [ ] File downloads when clicking the banner link
- [ ] Redirects to main site after ~1.5 seconds
- [ ] Works on Chrome, Firefox, Safari, Edge
- [ ] Works on mobile (iOS/Android)
- [ ] "Back" button doesn't return to this page (thanks to `location.replace`)
