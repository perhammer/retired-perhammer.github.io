---
title: "Java: Initialising complex types at declaration"
excerpt_separator: <!--more-->
---
One of the many things I like about Python is it's expressiveness, especially around initialising complex types. This made me look into ways of achieving the same thing in Java.
<!--more-->
Most programmers will be aware of initialising arrays at declaration:
```java
    String[] anArray = {"One", "Two", "Three"};
```

Using the ```java.util.Arrays``` class, you can quickly do the same thing for Lists too:
```java
    List<String> aList = Arrays.asList( 
            new String[]{"One", "Two", "Three"} 
    );
```

But what about more complex types like Maps and Sets?

Well, first you define an anonymous inner subclass of the complex type, and then put your initialisation code in the instance constructor:
```java
    Map<String, String> aMap = new HashMap<String, String>() {
       {
           put("One", "1");
           put("Two", "2");
           put("Three", "3");
       }
   };
```

Note that while your variable can be an abstract type ('Map' in this case), your anonymous declaration must extend a concrete implementation ('HashMap' in this case).

Using this, we can also initialise a Set -
```java
    Set<String> aSet = new HashSet<String>() {
       {
           add("One");
           add("Two");
           add("Three");
       }
   };
```

And finally, if you don't want to use the Arrays.asList(...) detour to initialise your List, you can do it using the same approach -
```java
    List<String> anotherList = new Vector<String>() {
       {
           add("One");
           add("Two");
           add("Three");
       }
   };
```

And that's how I initialise complex types at declaration in Java.