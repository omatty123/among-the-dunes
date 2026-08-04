# Among the Dunes - Publisher Submission Project

## CRITICAL RULE

**ACCURACY, ACCURACY, ACCURACY. DON'T MAKE SHIT UP.**

Before stating ANY fact: CHECK THE SOURCE FILES FIRST. If unsure, say "I don't know" and look it up. This is a professional publishing project.

---

## Quick Reference

- **Book:** Au Milieu des dunes / Among the Dunes
- **Author:** Louis Camara (Senegal)
- **Translator:** Dominica Chang
- **Award:** $4,000 PEN/Heim Translation Fund Grant

## Key URLs

- **Website:** https://omatty123.github.io/among-the-dunes/
- **Google Sheet:** https://docs.google.com/spreadsheets/d/1rFr_kkELlyQI4YOKCJ0J1_7xFfzhRHUQT5R7HkqHFWo/
- **Apps Script API:** https://script.google.com/macros/s/AKfycbzCn0OVw4BufZxOYv38_jNPzDS2uQXgGAEPyH28yBZyyQMng3qG4VMI4iwTwNA9fOXAPA/exec

## Main Files

- `index.html` - The website (all HTML, CSS, JS in one file)
- `PROJECT-NOTES.md` - Full documentation
- `Among_the_Dunes_Publisher_Database.xlsx` / `Email_Query_Templates.docx` - linked from the masthead

## Page Structure (rebuilt 2026-08-04)

Four tabs: **Publishers** (flat table, 20 rows, the landing view), **Query Letters** (4 templates),
**Tracker** (Sheet iframe), **Notes**. The old 9-section version (Overview, Pro Tips, Strategy,
Timeline, Downloads) was cut — publishers front and center, no pep talk.

## Status Sync — GOTCHA

Website dropdowns sync with the Google Sheet. Valid values must match exactly:
Not Started, Preparing, Contacted, Submitted, Waiting, Responded, Accepted, Rejected

**A row's `data-publisher` must equal the Sheet's column-A label EXACTLY or the status
silently never saves** — the sync `fetch` uses `mode: 'no-cors'`, so the Apps Script's
`{success:false}` is invisible in the browser. This bit us on Yale: the Sheet says
`Yale: Margellos`, the page displayed (and sent) `Yale: Margellos Series`, and every status
change on that row was lost with no error. The page now displays the long name but syncs the
short key. After adding a publisher, reconcile keys against the live API:
`curl -sL "$APPS_SCRIPT_URL"` and diff its keys against the page's `data-publisher` values.
