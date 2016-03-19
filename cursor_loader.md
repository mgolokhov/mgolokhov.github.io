---
layout: default
title:  Cursor Loaders
---

# Cursor Loaders

Content Provider has a very good friend, his name is CursorLoader, who does all hard work (runs a query) asynchronously in the background and reconnects to your Activity or Fragment when it's finished. With the help of CursorLoader a happy user doesn't see delay in the UI, or worse, "Application Not Responding" (ANR). Besides doing the initial background query, a CursorLoader automatically re-runs the query when data associated with the query changes.

CursorLoader class implements the [Loader](http://developer.android.com/reference/android/content/Loader.html) protocol in a standard way for querying cursors, building on [AsyncTaskLoader](http://developer.android.com/reference/android/content/AsyncTaskLoader.html) to perform the cursor query on a background thread. Implementing CursorLoader you'll bump into

* [LoaderManager](http://developer.android.com/reference/android/app/LoaderManager.html) - manages your Loaders for you. Responsible for dealing with the Activity or Fragment lifecycle.

* [LoaderManager.LoaderCallbacks](http://developer.android.com/reference/android/app/LoaderManager.LoaderCallbacks.html) - a callback interface for a client to interact with the LoaderManager.

Let's study how to use these classes and interfaces in an application.

# LoaderManager

LoaderManager is associated with an Activity or Fragment (host) for managing one or more Loader instances added to the manager. There is only one LoaderManager per host, but a LoaderManager can have multiple loaders. This class keeps your Loaders in line with the lifecycle of your host. If Android destroys your fragment or activity, the LoaderManager notifies the managed loaders to free up their resources. The LoaderManager is also responsible for retaining your data on configuration changes (e.g. orientation) and it calls the relevant callback methods when the data changes. You do not instantiate the LoaderManager yourself. Instead you simply call getLoaderManager() from within your host.

Most often you are only interested in two methods of the manager:

* initLoader
* restartLoader



