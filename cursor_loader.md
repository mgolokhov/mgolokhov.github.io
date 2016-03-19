---
layout: page
title:  "Cursor Loaders"
---

Content Provider has a very good friend, his name is CursorLoader, who does all hard work (runs a query) asynchronously in the background and reconnects to your Activity or Fragment when it's finished. With the help of CursorLoader a happy user doesn't see Application Not Responding (ANR) and delay in the UI. Besides doing the initial background query, a CursorLoader automatically re-runs the query when data associated with the query changes.