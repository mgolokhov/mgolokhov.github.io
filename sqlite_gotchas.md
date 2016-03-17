---
layout: page
title: SQLite gotchas
---

Here I would like to bookmark some tricky parts:

* SQLite has only [five data types](https://www.sqlite.org/datatype3.html):
null, integer, real, text and blob;
* SQLiteOpenHelper subclass expects Context and to avoid memory leaks use getApplicationContext();
* getReadableDatabase() and getWritableDatabase() are both writable, former get SQLiteDatabase without lock (hi threading);
* rawQuery() will be executed after you touch Cursor(e.g. moveToNext());
* [if the Cursor result set is over 1MB, it actually only holds a “window” on the data, and the story gets really really complicated](http://stackoverflow.com/a/31465262/5766983);
* you can use [RAWID instead of _id](https://www.sqlite.org/autoinc.html); 
* SQLite version matters and it depends on [Android version](http://stackoverflow.com/questions/2421189/version-of-sqlite-used-in-android);