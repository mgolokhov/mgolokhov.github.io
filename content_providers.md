---
layout: page
title: Content Providers
---

Keep on reading, if you answered “yes” to any of these questions:

* Do you have a structured set of data and want to share it across applications (processes)?
* Would you like to have access to contacts and the call log?
* Do you want to integrate your own search suggestions with the Android Quick Search Box?
* Would you like to implement App Widgets?
* Do you want a fancy syncing and querying data with auto-UI update?

<center><img src="{{ site.url }}/assets/content_provider.png"/></center>

Are you still with me? The following text describes content providers in more detail.

This topic is all about managing data (mostly stuctured, like DB). There are two sides ContentResolver (a client, data consumer) and ContentProvider. The provider object receives data requests from clients, performs the requested action, and returns the results.
Android itself manages data such as audio, video, images, personal contact information with the help of content providers a lot.

To work with data you'll expect basic CRUD (create, retrieve, update, delete) operations. And here you are: insert(), update(), delete(), and query() methods. Again, it smells like DB, but don't be fooled, content provider is just a layer (proxy) for data that lives somewhere else (SQlite database, on the file system, in flat files or on a remote server). 

Pretty simple and staightforward so far. But how do cliens find provider? Well, it's required to know the URIs of a provider to access it. The recomendation is to publish public constants for the URIs and document them to other developers. The URI has following format:

`scheme://authority/optional_data_type/optional_id`


Yeah, Google likes a **U**niform **R**esource **I**dentifiers that identify abstract or physical resources, as specified by [RFC 2396](http://www.ietf.org/rfc/rfc2396.txt). It's the most important concept to understand when dealing with content providers. Here is the detail of various parts:

* scheme - always set to `content://`

* authority  - a unique string used to locate your content provider and it should almost be a package name of your application, e.g. doit.study.droid.quizprovider

* optional_data_type  - the optional path, is used to distinguish the kinds of data your content provider offers. For example, if you are getting all the topics from the Quiz content provider, then the data path would be *topic* and URI would look like this content://doit.study.droid.quizprovider/topic, for *questions* - doit.study.droid.quizprovider/question

* optional_id - specifies the specific record requested. For example, if you are looking for topic number 5 in the Quiz content provider then URI would look like this content://doit.study.droid.quizprovider/topic/5

There are two types of URIs: directory- and id-based URIs:

<table>
	<tr>
		<th>Type</th>
		<th>Usage</th>
		<th>Constant</th>
	</tr>
	<tr>
		<td>vnd.android.cursor.item</td>
		<td>Used for single records</td>
		<td>ContentResolver.CURSOR_ITEM_BASE_TYPE</td>
	</tr>
	<tr>
		<td>vnd.android.cursor.dir</td>
		<td>Used for multiple records</td>
		<td>ContentResolver.CURSOR_DIR_BASE_TYPE</td>
	</tr>
</table>

When a request is made via a ContentResolver the Andoid OS inspects the authority of the given URI and passes the request to the content provider registered with that authority (finds a match in the AndroidManifest.xml). The content provider can interpret the rest of the URI however it wants. Actually, it can look like traditional URIs with keys and values:
`optional_data_type/sub_type?key=value`

To make your life easier there is the [UriMatcher](http://developer.android.com/reference/android/content/UriMatcher.html) class that parses URIs. You will tied each URI type to the constant integer with method

```java
addURI(String authority, String path, int code)
```


Are you thrilled to write your own Content Provider? It's very easy if you have already put data in DB. Do simple steps:

* extend ContentProvider base class
* define your URI address
* implement CRUD operations (override query methods)
* register your Content Provider in your AndroidManifest.xml file using &lt;provider&gt; tag.


Here is the list of query methods you should override:

<table>
	<tr>
		<td>
			<a href="http://developer.android.com/reference/android/content/ContentProvider.html#onCreate()/">onCreate()</a>
		</td>
		<td>is called when the provider is started.</td>
	</tr>
	<tr>
		<td>
			<a href="http://developer.android.com/reference/android/content/ContentProvider.html#query(android.net.Uri, java.lang.String[], java.lang.String, java.lang.String[], java.lang.String, android.os.CancellationSignal))/">query()</a>
		</td>
		<td>receives a request from a client and the result is returned as a Cursor object.</td>
	</tr>
	<tr>
		<td>
			<a href="http://developer.android.com/reference/android/content/ContentProvider.html#insert(android.net.Uri, android.content.ContentValues)/">insert()</a>
		</td>
		<td>inserts a new record into the content provider.</td>
	</tr>
	<tr>
		<td><a href="http://developer.android.com/reference/android/content/ContentProvider.html#delete(android.net.Uri, java.lang.String, java.lang.String[])/">delete()</a>
		</td>
		<td>deletes an existing record from the content provider.</td>
	</tr>
	<tr>
		<td><a href="http://developer.android.com/reference/android/content/ContentProvider.html#update(android.net.Uri, android.content.ContentValues, java.lang.String, java.lang.String[])/">update()</a>
		</td>
		<td>updates an existing record from the content provider.</td>
	</tr>
	<tr>
		<td><a href="http://developer.android.com/reference/android/content/ContentProvider.html#getType(android.net.Uri)/">getType()</a>
		</td>
		<td>returns the MIME type of the data at the given URI. The returned type should start with vnd.android.cursor.item for a single record, or vnd.android.cursor.dir/ for multiple items</td>
	</tr>
</table>

## Next

Check <a href="/cursor_loader">the best friend of Content Provider</a>




 

 

 
