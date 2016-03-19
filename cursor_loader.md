---
layout: default
title:  "Cursor Loaders"
---

Content Provider has a very good friend, his name is CursorLoader, who does all hard work (runs a query) asynchronously in the background and reconnects to your Activity or Fragment when it's finished. With the help of CursorLoader a happy user doesn't see delay in the UI or worse Application Not Responding (ANR). Besides doing the initial background query, a CursorLoader automatically re-runs the query when data associated with the query changes.

CursorLoader class implements the [Loader](http://developer.android.com/reference/android/content/Loader.html) protocol in a standard way for querying cursors, building on [AsyncTaskLoader](http://developer.android.com/reference/android/content/AsyncTaskLoader.html) to perform the cursor query on a background thread. Implementing CursorLoader you'll bump into

* LoaderManager - manages your Loaders for you. Responsible for dealing with the Activity or Fragment lifecycle

* LoaderManager.LoaderCallbacks - a callback interface for a client to interact with the LoaderManager

Let's study how to use these classes and interfaces in an application.

