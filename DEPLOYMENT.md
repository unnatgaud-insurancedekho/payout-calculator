# Deployment Guide — Payout Calculator

## Overview
This is a static HTML calculator that fetches rates from Google Sheets and logs calculations via Google Apps Script. It can be hosted on any static hosting platform (Vercel, Netlify, GitHub Pages, etc.).

---

## Prerequisites Checklist

### ✅ Google Sheets Setup
- [ ] **Rates Master Sheet** (ID: `1HUuQieygjV0pF2oGUJmjeeas1feF8UZ9Ijm_EyV7dSQ`)
  - Tab name: "Master Data"
  - Columns: Insurer | Plan | Plan Type | (spare) | PPT/PT | FYC Off | SYC Off | Total Off | Online Avail | FYC On | SYC On | Total On | Frequency
  - **Sharing**: Public (Anyone with link can view) via Share → Change to "Viewer"

- [ ] **Leads Log Sheet** (ID: `19VtHwIS8RC7szrZu3TqaLEyKXSR7kF2cQI36B2WMYo4`)
  - Tab name: "Sheet1" (or update the Apps Script code)
  - Will be auto-populated with each calculation
  - Header row: Timestamp | GID Code | Insurer | Plan Name | Plan Type | PPT/PT | Frequency | Rate Type | Premium ₹ | FYC % | FYC ₹ | SYC % | SYC ₹ | Total % | Total ₹

### ✅ Google Apps Script (2-minute setup)
1. Open the Leads sheet → **Extensions → Apps Script**
2. Replace all code with:

```javascript
function doGet(e) {
  const sheet = SpreadsheetApp.openById('19VtHwIS8RC7szrZu3TqaLEyKXSR7kF2cQI36B2WMYo4')
                               .getSheetByName('Sheet1');
  const p = e.parameter;
  sheet.appendRow([
    p.timestamp, p.gid, p.insurer, p.plan, p.planType,
    p.ppt, p.freq, p.rateType,
    Number(p.premium),
    Number(p.fycPct),  Number(p.fycAmt),
    Number(p.sycPct),  Number(p.sycAmt),
    Number(p.totalPct),Number(p.totalAmt)
  ]);
  return ContentService.createTextOutput('ok');
}
```

3. **Deploy** → New deployment → Web app
   - Execute as: (your email)
   - Who has access: **Anyone**
4. Copy the deployment URL (looks like: `https://script.google.com/macros/d/...`)
5. In `payout-calculator.html`, line 475:
   ```javascript
   const APPS_SCRIPT_URL = 'YOUR_APPS_SCRIPT_URL_HERE';
   ```

---

## Hosting Options

### Option 1: Vercel (Recommended)
```bash
# 1. Install Vercel CLI
npm i -g vercel

# 2. Login & deploy
vercel
# It will prompt you to link the repo and deploy automatically

# 3. It auto-redeploys on every push to main
```

**Pros**: Auto-deploy, free tier is generous, fast CDN, HTTPS included

### Option 2: Netlify
```bash
# 1. Connect GitHub repo at netlify.com
# 2. Set build command: (none — it's static)
# 3. Set publish directory: . (root)

# Or via CLI:
npm i -g netlify-cli
netlify deploy --prod
```

### Option 3: GitHub Pages
```bash
# 1. Go to repo Settings → Pages
# 2. Source: Deploy from a branch
# 3. Branch: main
# 4. Auto-deploys on every push
```

**Pros**: Free, no setup needed  
**Cons**: No custom domain without paid plan (or use freenom)

---

## Step-by-Step Deployment (Vercel)

1. **Ensure Apps Script URL is in the code** (line 475 of `payout-calculator.html`)

2. **Commit & push**:
   ```bash
   git add .
   git commit -m "Ready for deployment: Apps Script URL configured"
   git push origin main
   ```

3. **Deploy**:
   ```bash
   npm i -g vercel
   vercel
   ```

4. **Test**:
   - Open the provided URL
   - Try entering a GID (e.g., "GID-001")
   - Verify data loads from the Rates sheet
   - Complete a calculation
   - Check that the Leads sheet received a new row

---

## Verification Checklist

After deployment, test these flows:

- [ ] Page loads without errors (check browser console)
- [ ] "Loading rate data…" turns green with count (e.g., "45 rates loaded")
- [ ] Can select insurer → plan → ppt → frequency → rate type → premium
- [ ] PDF download button works
- [ ] After calculating, check Leads sheet for the new row
- [ ] All values are correct (GID, premium, FYC/SYC amounts)

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Failed to load — check sheet sharing" | Make sure Rates sheet is public (Share → Anyone with link) |
| Calculation not logging to Leads sheet | Verify Apps Script URL is correct in line 475 |
| Apps Script says "Authorization required" | Re-deploy as Web app and set "Who has access" to Anyone |
| PDF button disabled | Wait for rates to fully load (green status with count) |

---

## Files in This Repo

- `payout-calculator.html` — Main app (self-contained, all CSS/JS inline)
- `payout-calculator 2.html` — Backup copy
- `.gitignore` — (add if needed)
- `vercel.json` — Static hosting config (included)

---

## Security Notes

- ✅ No backend secrets exposed (Apps Script URL is public by design)
- ✅ Google Sheets are read-only for the app (rates are published)
- ✅ Logging to Sheets is authenticated via Apps Script (user identity not logged)
- ✅ Premium amounts are not sensitive (user provides them)

If you need to restrict the Rates sheet in future, use a Google API key and embed it (less secure but more restrictive).

---

## Next Steps

1. ✅ Set up Apps Script (copy the doGet function)
2. ✅ Paste Apps Script URL into line 475
3. ✅ Commit
4. ✅ Deploy to Vercel (or your chosen host)
5. ✅ Test the full flow
6. ✅ Share the live URL
