# Quick Deployment Checklist

## What's Ready ✅
- [x] HTML app fully built (payout-calculator.html)
- [x] Google Sheets IDs configured
- [x] Vercel config ready (vercel.json)
- [x] Deployment guide written (DEPLOYMENT.md)

---

## What You Need To Do (5 minutes)

### 1️⃣ Set Up Google Apps Script Logging (2 min)
- [ ] Open your Leads sheet: https://docs.google.com/spreadsheets/d/19VtHwIS8RC7szrZu3TqaLEyKXSR7kF2cQI36B2WMYo4
- [ ] Click **Extensions → Apps Script**
- [ ] Paste the code from DEPLOYMENT.md (the `doGet` function)
- [ ] Click **Deploy → New deployment → Web app**
- [ ] Set "Who has access" to **Anyone**
- [ ] **Copy the URL** it gives you

### 2️⃣ Add Apps Script URL to HTML
- [ ] Open `payout-calculator.html`
- [ ] Find line 475: `const APPS_SCRIPT_URL = '';`
- [ ] Paste your Apps Script URL between the quotes
- [ ] Save

### 3️⃣ Verify Google Sheets Sharing
- [ ] Rates sheet (1HUuQieygjV0pF2oGUJmjeeas1feF8UZ9Ijm_EyV7dSQ): **Share → Anyone with link → Viewer**
- [ ] Leads sheet (19VtHwIS8RC7szrZu3TqaLEyKXSR7kF2cQI36B2WMYo4): Apps Script has permission (it runs as you)

### 4️⃣ Deploy
- [ ] Commit: `git add . && git commit -m "Configure Apps Script URL for production"`
- [ ] Push: `git push origin main`
- [ ] Vercel auto-deploys on push OR manually: `vercel --prod`

### 5️⃣ Test
- [ ] Open the live URL
- [ ] Enter a GID code
- [ ] Select options through to the end
- [ ] Check that your Leads sheet received a new row with the calculation

---

## Deploy Instantly

```bash
# Make sure you've completed steps 1-2 above first, then:

git add .
git commit -m "Configure for production"
git push origin main

# Vercel auto-deploys, or manually:
vercel --prod
```

---

## Links
- 📊 Rates Sheet: https://docs.google.com/spreadsheets/d/1HUuQieygjV0pF2oGUJmjeeas1feF8UZ9Ijm_EyV7dSQ
- 📝 Logs Sheet: https://docs.google.com/spreadsheets/d/19VtHwIS8RC7szrZu3TqaLEyKXSR7kF2cQI36B2WMYo4
- 📖 Full Guide: See DEPLOYMENT.md

---

## Done? 🎉
Your app will be live at a URL like: `https://payout-calculator-xxx.vercel.app`

Every calculation will automatically log to your Leads sheet with timestamp and GID.
