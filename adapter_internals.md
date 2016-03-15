---
layout: page
title: Adapter Internals
---

First of all an adapter is a software design pattern that allows the interface of an existing class to be used from another interface. The adapter helps two incompatible interfaces to work together, so existing class works with others without modifying their source code.

In Android world the adapter an interface and the implementation will act as a bridge between a set of data and the AdapterView that displays the data. 

![smiley](https://cloud.githubusercontent.com/assets/294512/13728497/ef03d144-e92b-11e5-9fad-d128b48d5cbb.jpeg)

As you check [documentation](http://developer.android.com/reference/android/widget/AdapterView.html) you will see that ListView, GridView, Spinner (and many more) are subclasses of AdapterView.

In other words the adapter transforms your objects to something that can be viewed on the screen.

When you dig deeper into the API, you'll realise there are quite a few Adapter interfaces, abstract classes and implemented classes and it's a good idea to analyse it in conjunction with AdapterView hierarchy.

![smiley](https://cloud.githubusercontent.com/assets/294512/13728696/bb4b6f78-e931-11e5-8332-5c929153387a.png)
![smiley](https://cloud.githubusercontent.com/assets/294512/13730271/0dfcb450-e95c-11e5-9f31-8c9b344aa53c.png)

There are two sub-interfaces of Adapter: ListAdapter and SpinnerAdapter, which "bridges" data to ListViews (GridViews as well) and SpinnerViews respectively. Abstract BaseAdapter is a common base class of implementation for an Adapter that can be used in both ListView (by implementing the specialized ListAdapter interface) and Spinner (by implementing the specialized SpinnerAdapter interface). BaseAdapter already provides common implementations and at a minimum, you will need to implement four methods. 

<table>
<tr>
    <td>int getCount()</td>
    <td>How many items are in the data set represented by this Adapter.</td>
</tr>
<tr>
    <td>Object getItem(int position)</td>
    <td>Get the data item associated with the specified position in the data set.</td>
</tr>
<tr>
    <td>long getItemId(int position)</td>
    <td>Get the row id associated with the specified position in the list.</td>
</tr>
<tr>
    <td>View getView(int position, View convertView, ViewGroup parent)</td>
    <td>Get a View that displays the data at the specified position in the data set.</td>
</tr>
</table>

What's about other adapters? In general, they provide even more convenience (less code), but may expect specific views and data types.

An ArrayAdapter is an adapter backed by an array of objects. It links the array to the Adapter View.
The default ArrayAdapter converts an array item into a String object putting it into a TextView. The text view is then displayed in the AdapterView (e.g. a ListView).
When you create the adapter, you need to supply the layout for displaying each array string. You can define your own or use one of Android’s, such as: [android.R.layout.simple_list_item_1](https://android.googlesource.com/platform/frameworks/base.git/+/android-6.0.1_r22/core/res/res/layout/simple_list_item_1.xml).

The SimpleAdapter links static data to views defined in a layout file. You specify the data as an ArrayList of Maps. Each entry in the ArrayList will display as a row in a list.

The CursorAdapter (and its subclass the SimpleCursorAdapter) utilizes an Android Cursor (often from an SQLite database or ContentProvider) to provide the data set for the Adapter.

---------------------

## References

[https://en.wikipedia.org/wiki/Adapter_pattern](https://en.wikipedia.org/wiki/Adapter_pattern)

[http://www.intertech.com/Blog/android-adapters-adapterviews](http://www.intertech.com/Blog/android-adapters-adapterviews)