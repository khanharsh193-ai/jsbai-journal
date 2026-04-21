# JSBAI — Setup Guide
## Make the site fully live in ~15 minutes

---

## What you get after setup

✅ Real database — every submission saved to Google Sheets permanently  
✅ File storage — manuscripts, figures, cover letters saved to Google Drive  
✅ Automatic emails — author gets confirmation, you get notified on every submission  
✅ Admin dashboard — manage submissions, update status, add notes  
✅ Total cost: ₹0  

---

## Step 1 — Set up the Google Apps Script backend

This is the server that receives submissions, saves files, and sends emails.
It runs on Google's servers for free using your Google account.

1. Go to **[script.google.com](https://script.google.com)**
2. Click **New project**
3. Delete all the default code in the editor
4. Open the file `Code.gs` from this folder, copy **all** the content
5. Paste it into the Apps Script editor
6. At the top of the pasted code, find the `CONFIG` block and fill in:
   ```javascript
   EDITOR_EMAIL:   'your.actual@gmail.com',   // your Gmail
   ADMIN_PASSWORD: 'choose-a-strong-password', // admin panel password
   ```
7. Click the **floppy disk icon** (Save) — name the project `JSBAI Backend`
8. Click **Deploy → New deployment**
9. Click the gear icon next to "Type" → select **Web app**
10. Set:
    - Description: `JSBAI Backend v1`
    - Execute as: **Me**
    - Who has access: **Anyone**
11. Click **Deploy**
12. Click **Authorize access** → choose your Google account → click **Allow**
13. Copy the **Web app URL** — it looks like:  
    `https://script.google.com/macros/s/AKfycb.../exec`

---

## Step 2 — Paste the URL into both HTML files

Open `index.html` in any text editor (Notepad, VS Code, etc.)

Find this line near the top of the `<script>` block:
```javascript
const SCRIPT_URL = 'YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL';
```

Replace `'YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL'` with your actual URL:
```javascript
const SCRIPT_URL = 'https://script.google.com/macros/s/AKfycb.../exec';
```

Save `index.html`.

Now open `admin.html`. Find the same two lines:
```javascript
const SCRIPT_URL     = 'YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL';
const ADMIN_PASSWORD = 'jsbai-admin-2025';
```

Paste the same URL. Change `ADMIN_PASSWORD` to match what you set in `Code.gs`.

Save `admin.html`.

---

## Step 3 — Push to GitHub

```bash
git add index.html admin.html
git commit -m "Connect backend"
git push
```

That's it. Your site is live.

---

## How to use the Admin Panel

Go to: `https://yourusername.github.io/yourrepo/admin.html`

Login:
- Username: anything (decorative)
- Password: what you set in Step 1

From the admin panel you can:
- **See all submissions** — title, author, affiliation, keywords, abstract
- **Download files** — click Manuscript / Cover Letter / Figures links (opens Google Drive)
- **Change status** — use the dropdown: Under Review → Revision Requested → Accepted → Published
- **Add private notes** — editorial comments visible only to you
- **Search** — by title, author name, email, ref ID, or affiliation
- **Filter** — by status or article type

---

## Where your data lives

| Data | Location |
|---|---|
| All submission records | Google Sheets → "JSBAI Submissions" |
| Manuscript files | Google Drive → "JSBAI Manuscript Files" folder |
| Emails sent | Your Gmail Sent folder |

To find your Google Sheet: go to [drive.google.com](https://drive.google.com) and search for "JSBAI Submissions".

---

## If something doesn't work

**Submissions not saving?**
- Make sure you re-deployed after changing `EDITOR_EMAIL` in Code.gs
- Every time you edit Code.gs, go to Deploy → Manage deployments → edit → update version

**Emails not arriving?**
- Check your Gmail spam folder
- Make sure `EDITOR_EMAIL` in Code.gs matches your actual Gmail address

**Admin panel shows "Error loading data"?**
- Double-check `SCRIPT_URL` in `admin.html` — should be identical to `index.html`
- Make sure `ADMIN_PASSWORD` in `admin.html` matches `ADMIN_PASSWORD` in `Code.gs`

**CORS error in browser console?**
- The `redirect: 'follow'` in the fetch call handles Apps Script redirects automatically
- Make sure you copied the full URL including `/exec` at the end
