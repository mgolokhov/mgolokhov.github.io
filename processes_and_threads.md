---
layout: page
title: Processes and threads
---

Android uses Linux processes with the single thread of execution, "main" thread (UI thread).

By default, all components of the same application starts and live in this thread.

Default behavior can be change in manifest (with android:process).

Even components from different applications can share one process.

Android OS cannot kill Activities, but might shut down a process at some point (ex. low memory). 

Long running operations can block UI thread as the result after 5 sec user gets  "application not responding" (ANR).

Andoid UI toolkit is not thread-safe (don't access the UI from outside the UI thread)

Developer can create other threads ("background" or "worker" threads).

Variants:

### Standart java SE threads + helper methods to post result in the UI thread

* Activity.runOnUiThread(Runnable)
* View.post(Runnable)
* View.postDelayed(Runnable, long)

### AsyncTask





