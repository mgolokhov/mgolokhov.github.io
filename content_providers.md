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

`scheme://authority/data_type/id`


Yeah, Google likes URIs and URLs :) Here is the detail of various parts:

* scheme - always set to `content://`

* authority  - a unique string used to locate your content provider and it should almost be a package name of your application, e.g. doit.study.droid.quizprovider

* data_type  - indicates the type of data that this particular provider provides. For example, if you are getting all the topics from the Quiz content provider, then the data path would be topic and URI would look like this content://doit.study.droid.quizprovider/topic

* id - specifies the specific record requested. For example, if you are looking for topic number 5 in the Quiz content provider then URI would look like this content://doit.study.droid.quizprovider/topic/5

Are you thrilled to write your own Content Provider? It's very easy if you have already put data in DB. Do simple steps:

* extend ContentProvider base class
* define your URI address
* implement CRUD operations (override query methods)
* register your Content Provider in your AndroidManifest.xml file using <provider> tag.


Here is the list of query methods you should override:

<table>
	<tr>
		<td>onCreate()</td>
		<td>is called when the provider is started.</td>
	</tr>
	<tr>
		<td>query() </td>
		<td>receives a request from a client and the result is returned as a Cursor object.</td>
	</tr>
	<tr>
		<td>insert()</td>
		<td>inserts a new record into the content provider.</td>
	</tr>
	<tr>
		<td>delete()</td>
		<td>deletes an existing record from the content provider.</td>
	</tr>
	<tr>
		<td>update()</td>
		<td>updates an existing record from the content provider.</td>
	</tr>
	<tr>
		<td>getType()</td>
		<td>returns the MIME type of the data at the given URI.</td>
</table>





 

 

 
