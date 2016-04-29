---
layout: default
title:  Cursor Loaders
position: 6
---

# Cursor Loaders

Content Provider has a very good friend, his name is CursorLoader, who does all hard work (runs a query) asynchronously in the background and reconnects to your Activity or Fragment when it's finished. With the help of CursorLoader a happy user doesn't see delay in the UI, or worse, "Application Not Responding" (ANR). Besides doing the initial background query, a CursorLoader automatically re-runs the query when data associated with the query changes.

CursorLoader class implements the [Loader](http://developer.android.com/reference/android/content/Loader.html) protocol in a standard way for querying cursors, building on [AsyncTaskLoader](http://developer.android.com/reference/android/content/AsyncTaskLoader.html) to perform the cursor query on a background thread. Implementing CursorLoader you'll bump into

* [LoaderManager](http://developer.android.com/reference/android/app/LoaderManager.html) - manages your Loaders for you. Responsible for dealing with the Activity or Fragment lifecycle.

* [LoaderManager.LoaderCallbacks](http://developer.android.com/reference/android/app/LoaderManager.LoaderCallbacks.html) - a callback interface for a client to interact with the LoaderManager.

Let's study how to use these classes and interfaces in an application.

### LoaderManager

LoaderManager is associated with an Activity or Fragment (host) for managing one or more Loader instances added to the manager. There is only one LoaderManager per host, but a LoaderManager can have multiple loaders. This class keeps your Loaders in line with the lifecycle of your host. If Android destroys your fragment or activity, the LoaderManager notifies the managed loaders to free up their resources. The LoaderManager is also responsible for retaining your data on configuration changes (e.g. orientation) and it calls the relevant callback methods when the data changes. You do not instantiate the LoaderManager yourself. Instead you simply call getLoaderManager() from within your host.

Most often you are only interested in two methods of the manager:

* [Loader&lt;Cursor&gt; initLoader (int id, Bundle args, LoaderCallbacks&lt;Cursor&gt; callback)](http://developer.android.com/reference/android/app/LoaderManager.html#initLoader(int, android.os.Bundle, android.app.LoaderManager.LoaderCallbacks<D>))
* [Loader&lt;Cursor&gt; restartLoader (int id, Bundle args, LoaderCallbacks&lt;Cursor&gt; callback)](http://developer.android.com/reference/android/app/LoaderManager.html#restartLoader(int, android.os.Bundle, android.app.LoaderManager.LoaderCallbacks<D>))

### initLoader()

This method will create a new Loader if it does not exist, otherwise re-use the last created with the same *id*. 
As the name suggests the primary function is to initialize the loader. It also starts the loader if the activity/fragment is running (resumed).

*int id* - a unique identifier for this loader.

*Bundle args*  - optional arguments used only to construct a new loader.

*LoaderCallbacks &lt;Cursor&gt; callback* - required interface used to report about changes in the state of the loader

### restartLoader()

Because Android doesn’t execute the query again, you need a way to re-initialize the Loader when data, that is used to build the query, changes. Typical examples are search queries. Arguments are the same as in initLoader()

### LoaderManager.LoaderCallbacks

Callback interface for a client to interact with the manager. It defines methods you must implement to create your Loader, to deal with the results and to clean up resources. These methods are:

* [onCreateLoader(int id, Bundle args)](http://developer.android.com/reference/android/app/LoaderManager.LoaderCallbacks.html#onCreateLoader(int, android.os.Bundle)) - Instantiate and return a new Loader for the given ID.
* [onLoadFinished(Loader&lt;Cursor&gt; loader, Cursor data)](http://developer.android.com/reference/android/app/LoaderManager.LoaderCallbacks.html#onLoadFinished(android.content.Loader&lt;Cursor&gt, Cursor)) - Called when a previously created loader has finished its load.
* [onLoaderReset (Loader&lt;Cursor&gt; loader)](http://developer.android.com/reference/android/app/LoaderManager.LoaderCallbacks.html#onLoaderReset(android.content.Loader<D>)) - Called when a previously created loader is being reset, thus making its data unavailable.

### onCreateLoader()

When the LoaderManager calls initLoader() it checkes a loader existance with a given id and calls back to onCreateLoader only to create a new one. Arguments are re-used from initLoader(). To build the CursorLoader using its constructor method, it requires the complete set of information needed to perform a query to the ContentProvider (specifically: uri, projection, selection, selectionArgs, sortOrder).

### onLoadFinished()

Here you update the UI based on the results of your query. Here you update the UI based on the results of your query. This method is guaranteed to be called prior to the release of the last data that was supplied for this loader. At this point you should remove all use of the old data (since it will be released soon), but should not do your own release of the data since its loader owns it and will take care of that (**no need to explicitly close cursors**).

### onLoadReset()

This method allows you to release any resources you hold, so that the Loader can free them. You can set any references to the cursor object you hold to null. And again **no need to explicitly close cursors**.

# References

[http://www.androiddesignpatterns.com/2012/07/understanding-loadermanager.html](http://www.androiddesignpatterns.com/2012/07/understanding-loadermanager.html)

[http://www.grokkingandroid.com/using-loaders-in-android/#how_to_deal_with_cursoradapters](http://www.grokkingandroid.com/using-loaders-in-android/#how_to_deal_with_cursoradapters)

[https://developer.android.com/guide/components/loaders.html](https://developer.android.com/guide/components/loaders.html)


