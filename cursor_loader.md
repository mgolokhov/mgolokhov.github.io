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

* [`Loader<Cursor> initLoader (int id, Bundle args, LoaderCallbacks<Cursor> callback)`](http://developer.android.com/reference/android/app/LoaderManager.html#initLoader(int, android.os.Bundle, android.app.LoaderManager.LoaderCallbacks<D>))
* [`Loader<Cursor> restartLoader (int id, Bundle args, LoaderCallbacks<Cursor> callback)`](http://developer.android.com/reference/android/app/LoaderManager.html#restartLoader(int, android.os.Bundle, android.app.LoaderManager.LoaderCallbacks<D>))

# initLoader()

This method will create a new Loader if it does not exist, otherwise re-use the last created with the same *id*. 
As the name suggests the primary function is to initialize the loader. It also starts the loader if the activity/fragment is running (resumed).

*int id* - a unique identifier for this loader.

*Bundle args*  - optional arguments used only to construct a new loader.

*LoaderCallbacks &lt;Cursor&gt; callback* - required interface used to report about changes in the state of the loader

# restartLoader()

Because Android doesn’t execute the query again, you need a way to re-initialize the Loader when data, that is used to build the query, changes. Typical examples are search queries. Arguments are the same as in initLoader()

# Some key points

With CursorLoader there is no need to explicitly close cursors.

# References

[http://www.androiddesignpatterns.com/2012/07/understanding-loadermanager.html](http://www.androiddesignpatterns.com/2012/07/understanding-loadermanager.html)

[http://www.grokkingandroid.com/using-loaders-in-android/#how_to_deal_with_cursoradapters](http://www.grokkingandroid.com/using-loaders-in-android/#how_to_deal_with_cursoradapters)



