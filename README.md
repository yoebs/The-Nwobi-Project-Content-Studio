# The Nwobi Project — Content Studio

Your personal short-form content engine. Open on Monday, Wednesday, or Friday to generate 4 fully structured video scripts — one per category — with hooks, sections, pattern interrupts, and reference links for b-roll, images, and YouTube examples.

---

## How To Deploy (GitHub Pages — Free)

### Step 1 — Create a GitHub account
Go to github.com and sign up if you don't have an account.

### Step 2 — Create a new repository
1. Click the **+** icon → **New repository**
2. Name it: `nwobi-content-studio`
3. Set to **Private** (important — keeps your code hidden)
4. Click **Create repository**

### Step 3 — Upload the app
1. Click **Add file → Upload files**
2. Drag and drop `index.html` from this folder
3. Click **Commit changes**

### Step 4 — Enable GitHub Pages
1. Go to your repo → **Settings** → **Pages** (left sidebar)
2. Under Source, select **Deploy from a branch**
3. Select branch: **main**, folder: **/ (root)**
4. Click **Save**
5. Wait 2-3 minutes. Your app URL will appear at the top of the Pages settings page.

Your app is now live at: `https://[your-username].github.io/nwobi-content-studio`

Bookmark this URL and open it every Monday, Wednesday, and Friday.

---

## How To Get Your Claude API Key

1. Go to **console.anthropic.com**
2. Sign in or create an account
3. Click **API Keys** in the left sidebar
4. Click **Create Key**
5. Name it: `Nwobi Content Studio`
6. Copy the key immediately — you cannot see it again
7. Save it to your password manager
8. Paste it into the app each session when prompted

---

## Google Sheets Integration (Optional)

The app can automatically log every video you mark as filmed to a Google Sheet. This gives Claude a persistent memory of what you've made so it can avoid repeating concepts across sessions.

### Step 1 — Create the Sheet
1. Go to **Google Sheets** → create a new spreadsheet
2. Name it: `Nwobi Content Log`
3. Add these headers in Row 1 exactly:
   `Date | Day | Category | Hook | Concept | Style | Filmed`

### Step 2 — Create the Apps Script
1. In your Sheet, go to **Extensions → Apps Script**
2. Delete any existing code
3. Paste this code:

```javascript
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data = JSON.parse(e.postData.contents);
    
    sheet.appendRow([
      data.dateStr || new Date().toLocaleDateString('en-GB'),
      new Date().toLocaleDateString('en-GB', { weekday: 'long' }),
      data.category,
      data.hook,
      data.concept,
      data.style,
      data.filmed ? 'Yes' : 'No'
    ]);
    
    return ContentService
      .createTextOutput(JSON.stringify({ success: true }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch(err) {
    return ContentService
      .createTextOutput(JSON.stringify({ error: err.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

4. Click **Save** (name the project `Nwobi Logger`)

### Step 3 — Deploy as Web App
1. Click **Deploy → New Deployment**
2. Click the gear icon next to **Type** → select **Web App**
3. Set **Execute as**: Me
4. Set **Who has access**: Anyone
5. Click **Deploy**
6. Copy the **Web App URL**

### Step 4 — Add URL to the App
1. Open your Content Studio app
2. Go to **Settings** tab
3. Paste the Web App URL into the **Google Sheets URL** field
4. Click **Save URL**

From now on, every video you mark as filmed will automatically log to your Sheet.

---

## How To Use The App

### On recording days (Monday, Wednesday, Friday):
1. Open the app URL
2. Enter your Claude API key when prompted
3. On the **Generate** tab, tap **Generate Today's Videos**
4. Claude searches for trending topics, checks your log, then generates 4 video structures
5. Tap each card to expand the full script — hook, story, lesson, pattern interrupts, action, CTA
6. Use the reference links to find b-roll, images, and YouTube clips for editing
7. When you've filmed a video, toggle **Mark as Filmed**
8. Use **Export PDF** to save the day's scripts for reference while filming

### Repeat protection:
- Claude checks your video log before generating
- If a concept is too similar to something you've already made, it flags it with a warning
- You can keep it with a different angle or regenerate that category

### Trend integration:
- Claude searches the web before every generation to find trending business news, new books, and recent psychology/mindset research
- Videos with a trending angle are tagged 🔥 in the app

---

## Updating the App

To edit the code later:
1. Go to your GitHub repo
2. Click `index.html`
3. Click the **pencil icon** (Edit)
4. Make your changes
5. Click **Commit changes**
6. GitHub Pages updates automatically within 2-3 minutes

---

## Categories

| Category | Format | Target feeling |
|---|---|---|
| Easy Daily Task | Direct, actionable. One task the viewer does today | "I can do that right now" |
| Motivational | Real named story → lesson → how they apply it | "If they did it, I can too" |
| Psychology | Named business example → principle → application | "I never knew that. I can use it." |
| Mindset | Real story of mindset building or destroying something | "I need to think about this differently" |

All videos: minimum 60 seconds final length. One hook, one story/proof, one lesson, one action, one CTA.
