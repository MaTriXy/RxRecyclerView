# NOTE

This library is DEPRECATED and not recommended for utilization, as it uses very old versions of Rx and Gradle.  However, I am leaving it here so that you might be able to draw on some of the concepts if you're implementing your own version, or for you to freely fork and modify / update.

Known issues:

The gradle version / bintray version are super duper old.  Please remove any references to bintray if you clone this yourself, or update them as necessary.

# RxRecyclerViewAdapter Library 2.0

Crazy easy to use RecyclerView Adapter for Reactive Applications

![Data Flow Visualization](http://i.imgur.com/SxDOIyB.png)

## Interface

* ```RxRecyclerViewAdapter::onCreateViewHolder``` is the same as ```RecyclerView.Adapter```
* ```RxRecyclerViewAdapter::onBindViewHolder``` gives you the Element you are
  binding to
* ```RxRecyclerViewAdapter::preProcessElement``` gives you the chance to work with elements before they enter the underlying tree set.
* ```RxRecyclerViewAdapter::postProcessElement``` gives you the chance to work with elements after the RecyclerView has been notified of the element.
* ```Event<K,V>``` is Immutable and takes an ```Event.TYPE```, Key, and Value.
* ```Event<K,V>``` also contains an UNKNOWN type and is overridable for custom
  processing.
* ```EventElement<K,V>``` and it's subclasses wrap Events and contain a view
  type for easy addition of headers, footers, and "list is empty" view.

## Creating an Adapter

You need to have an Observable that you've merged all of your event emitters
into.  You then need to either map those into EventElements or utilize
GroupComparator and it's corresponding Operator via Observable::lift to do the
conversion for you.

## Sorting and Grouping your stuff

There is an interface called ```GroupComparator``` that lets you sort and group your
Events.  These are passed to an instance of ```ElementGenerationOperator```
which will then add in Header and Footer items, as well as handle Empty items
per your provided Options.  The Adapter uses a TreeSet internally, which allows
for automatic sorting by natural keys (Elements subclass Comparator).

## View Types

You can create your own new view types by extending the appropriate EventElement subclass, or EventElement
itself.  Each View type has a corresponding bit mask that are placed in the 11th and 12th bits of the 
View type integer.  This means that when you want to know what kind of view you are looking at, you can
simply shift it's view type like so: ```element.getViewType() >> EventElement.MASK_SHIFT``` and compare it
to the defined static integer masks within ```EventElement```.  This allows you to do things like create
your own Header elements and whatnot, with unique viewtypes, and rest assured that they'll work properly.

This system allows us to avoid using instanceof calls everywhere, and stick to switch cases.

## Examples

Are available in the app module!

## Licensing

This work is (C) under the MIT License.

## Gradle

This has been released on Bintray

```groovy
repositories {
    maven {
        url  "http://dl.bintray.com/exallium/maven" 
    }
}

dependencies {
    compile 'com.exallium.rxrecyclerview:lib:2.1.2'
}
```
