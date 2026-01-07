# Dominica's Publishing Project: Among the Dunes

## IMPORTANT: Project Rules
**ACCURACY IS OF UTMOST IMPORTANCE.** This is a professional publishing project. Never fabricate or assume information - always verify by checking the source files first. If you don't know something, say so and look it up.

---

## Book Details
- **French Title:** Au Milieu des dunes
- **English Title:** Among the Dunes
- **Author:** Louis Camara (Senegal)
- **Translator:** Dominica Chang
- **Word Count:** ~70,000 words (translated)
- **Award:** $4,000 PEN/Heim Translation Fund Grant

---

## Live Website
**URL:** https://omatty123.github.io/among-the-dunes/

The website serves as a publisher submission guide and tracking tool.

### Features:
- Publisher list organized by priority (HIGH, MEDIUM, LOW)
- Status dropdown for each publisher (synced with Google Sheet)
- Notes section with auto-save to localStorage
- Direct links to publisher submission pages
- Sample chapters, synopsis, and translator bio sections

---

## Google Sheet Integration

### Google Sheet
**URL:** https://docs.google.com/spreadsheets/d/1rFr_kkELlyQI4YOKCJ0J1_7xFfzhRHUQT5R7HkqHFWo/

### Google Apps Script Web App
**URL:** https://script.google.com/macros/s/AKfycbzCn0OVw4BufZxOYv38_jNPzDS2uQXgGAEPyH28yBZyyQMng3qG4VMI4iwTwNA9fOXAPA/exec

### How the Sync Works:
1. Website loads → fetches current statuses from Google Sheet
2. User changes a dropdown → website sends update to Google Apps Script
3. Apps Script updates the corresponding row in the sheet
4. Changes persist in both places

### Valid Status Values (must match exactly):
- Not Started
- Preparing
- Contacted
- Submitted
- Waiting
- Responded
- Accepted
- Rejected

**Important:** The Google Sheet has data validation on the Status column. Any new status values must be added to both the website dropdowns AND the sheet's data validation rules.

---

## GitHub Repository
**URL:** https://github.com/omatty123/among-the-dunes

### To Update the Website:
```bash
cd /Users/wegehaum/among-the-dunes-site
# Make changes to index.html
git add .
git commit -m "Description of changes"
git push
```
Changes go live within 1-2 minutes on GitHub Pages.

---

## Google Apps Script Code

To edit: Open the Google Sheet → Extensions → Apps Script

```javascript
function doGet(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();

  if (e.parameter.action === 'debug') {
    var values = sheet.getDataRange().getValues();
    var publishers = [];
    for (var i = 1; i < values.length; i++) {
      publishers.push(String(values[i][0]));
    }
    return ContentService.createTextOutput(JSON.stringify({publishers: publishers}))
      .setMimeType(ContentService.MimeType.JSON);
  }

  if (e.parameter.action === 'update') {
    var publisher = e.parameter.publisher.trim();
    var status = e.parameter.status;
    var values = sheet.getDataRange().getValues();
    for (var i = 1; i < values.length; i++) {
      var sheetPublisher = String(values[i][0]).trim();
      if (sheetPublisher === publisher) {
        sheet.getRange(i + 1, 4).setValue(status);
        return ContentService.createTextOutput(JSON.stringify({success: true, row: i+1, matched: sheetPublisher}))
          .setMimeType(ContentService.MimeType.JSON);
      }
    }
    return ContentService.createTextOutput(JSON.stringify({success: false, searched: publisher}))
      .setMimeType(ContentService.MimeType.JSON);
  }

  // Default: return all statuses
  var data = sheet.getDataRange().getValues();
  var statuses = {};
  for (var i = 1; i < data.length; i++) {
    var pub = String(data[i][0]).trim();
    var stat = String(data[i][3]).trim() || 'Not Started';
    if (pub) statuses[pub] = stat;
  }
  return ContentService.createTextOutput(JSON.stringify(statuses))
    .setMimeType(ContentService.MimeType.JSON);
}
```

**After editing Apps Script:** Deploy → New deployment → Web app → Execute as "Me", access "Anyone"

---

## Publisher List (as of project creation)

### HIGH Priority
- Archipelago Books
- Deep Vellum Publishing
- Iskanchi Press
- Open Letter Books (Opens June)
- Two Lines Press
- University of Georgia Press

### MEDIUM Priority
- Transit Books
- New Vessel Press
- Tilted Axis Press
- Comma Press
- Guernica Editions
- Arsenal Pulp Press
- Balestier Press
- Feminist Press at CUNY
- Yale: Margellos
- Restless Books

### LOW Priority
- Peirene Press
- Dedalus Books
- Coimpress
- New Directions

---

## Troubleshooting

### Status not syncing to Google Sheet?
1. Check that the status value matches exactly (case-sensitive)
2. Verify data validation in sheet includes that status
3. Check Apps Script deployment is current version
4. Test API directly: `[SCRIPT_URL]?action=update&publisher=Test&status=Contacted`

### Website not updating after push?
- Wait 1-2 minutes for GitHub Pages cache
- Hard refresh browser (Cmd+Shift+R)

### Notes not saving?
- Notes save to browser localStorage
- Different browsers/devices have separate storage
- Clear localStorage to reset: `localStorage.clear()`

---

## Contact
Email notifications configured to: omatty@gmail.com
