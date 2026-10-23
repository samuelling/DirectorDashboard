# DirectorDashboard

Private-data dashboard for the BBC PhD program director: tasks, per-student milestones, notes, and live annual-report status.

- `index.html` is the whole app and contains **no student data**. It unlocks with a director key (open it once as `.../#KEY`; the key is then remembered on that device and removed from the URL).
- Data lives in a private Google Sheet ("BBC Director Dashboard (data)") written by a Google Apps Script web app (`ENDPOINT` in `index.html`). The script holds the key and roster and is deliberately not stored in this repo.
- Annual-report status is read from the BBC Progress Reports sheet by the same script.
