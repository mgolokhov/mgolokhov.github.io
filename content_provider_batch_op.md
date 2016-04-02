---
layout: page
title: Batch operations for Content Provider
---

[http://www.grokkingandroid.com/better-performance-with-contentprovideroperation/](http://www.grokkingandroid.com/better-performance-with-contentprovideroperation/)

[http://www.programering.com/a/MDM2kDMwATU.html](http://www.programering.com/a/MDM2kDMwATU.html)

[http://developer.android.com/guide/topics/providers/content-provider-basics.html#Batch](http://developer.android.com/guide/topics/providers/content-provider-basics.html#Batch)


The list of ContentProviderOperations has to be an ArrayList.

### bulkInsert() vs applyBatch()

"With bulkInsert() you can only insert to the same URI (usually representing one table in the backing database). With applyBatch() you can use different operations (delete, update and insert) and apply them for different URIs. Every operation can use its own URI.
Thus you could first insert something to one table and then something to depending tables (say a music album and it’s titles respectively). You can also use the id of the first insert (of the music album) for the other inserts (of the music titles) – which isn’t possible with bulkInsert() as well."