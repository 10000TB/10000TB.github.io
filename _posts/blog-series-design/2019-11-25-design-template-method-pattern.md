---
layout: "post"
title: "Template Method Pattern - Explained form a day-to-day work example"
categories: design
tags: [design, pattern, python]
---


This morning, I was working with a colleague on a code review, and I was suggested to adopt
"Template Method DP" for my property calculation in a python class. I was not familiar with
this design pattern as all. It aroused my curiosity and I went on to search and read more about
it. I then thought it might be worth blogging about it.

### The code of interest

Without mentioning confidential information, here is an example code snippet I was discussing
with the colleague:

```python

class SomeUtilClass(object):
    
    def __init__(self):
        self._property_a = None
        self._property_b = None

    @property
    def property_a(self):
        if not self._property_a:
            # For demo only, this line does not compile.
            self._property_a = SomeMagicClass.newRpcStub(...)
        return self._property_a

    @property
    def property_b(self):
        if not self._property_b:
            # For demo only, this line does not compile.
            self._property_b = SomeOtherWonderfulClass.newServiceClient(...) 
        return self._property_b 
```

### The problems explaind

Note that there is nothing syntactically wrong about the code above. From styling and readability wise, it was pointed
out that the code above is worth improvement. Specifically, if the class is intended to be inherited, having property
value calculation all in property decorated function can make property override complex and messy in subclasses.

One such example is when one intend to modify the getter of `property_a` in subclass of `SomeSubClass`, they would need to
create an indenpendent new property in `SomeSubClass`, they would need to rewrite all logic from scratch, and they end
up creating a new property, no inheritance at all, why subclassing ? The above code snippet is bad as a result.

In addition, if one need to override setter of `property_a`, they would have to reference the property from super class:

```python
class SomeSubClass(SomeUtilClass):

    ... # omit unrelated subclass stuff.

    @SomeUtilClass.property_a.setter
    def property_a(self, v)
        self._property_a = v

    ... # omit other unrelated stuff.
```

From what it looks like, it is not clean. Due to python's namespacing rules, one has to reference the property from its super
class to avoid compilation error. Taking a step back, we accept what it looks. This works well if we are adding a new setter
for property property_a. However, if we are overriding an exising setter from parent class in subclass, the parent class's
setter's behavior will also be modified, which is probably not desired.

### Template method pattern for the win

Template method pattern works well here. One can move property calculation into a separate method. Overriding this calculation
method in subclass solves all problems in a clean way. Here is what it looks like:

```python

class SomeUtilClass(object):
    
    def __init__(self):
        self._property_a = None
        self._property_b = None

    @property
    def property_a(self):
        return self._get_property_a()

    # The calculation method.
    def _get_property_a(self):
       """Indirect accessor of property_a"""
        if not self._property_a:
            # For demo only, this line does not compile.
            self._property_a = SomeMagicClass.newRpcStub(...)
        return self._property_a

    @property
    def property_b(self):
        return self._get_property_b()

    # The calculation method.
    def _get_property_b(self):
        """Indirect accessor of property_b"""
        if not self._property_b:
            # For demo only, this line does not compile.
            self._property_b = SomeOtherWonderfulClass.newServiceClient(...)
        return self._property_b 
        
```

In this approach, property override in subclassing is made easy, as you only need to override the calculation method:

```python
class SomeUtilClass(object):
    
    ... # omitting unrelated stuff - this does not compile.

    # The calculation method.
    def _get_property_a(self):
       """Indirect accessor of property_a"""
        # Some new logic to access _property_a .
        return self._property_a

    ... # omitting some other unrelated stuff.
```

In principle, Template method pattern says that methods in super class define series of behaviors and template method put
together calls to those method, which defines core algorithm, and which is not called nor changed. In subclasses
 of different flavors, they simply implement/override those individual steps methods. An example of deisgn looks like follow:

(This pic taken from internet is makred as Java, but the principle is the same)
![template method pattern](/assets/img/blog-series-design/template_method_design_pattern_example.png)
>Image credit: https://stacktips.com/tutorials/design-patterns/template-method-design-pattern-in-java)

From the same source of the image, a templated pizza class does a great job demonstrating the principle::

```java
// Code referenced from https://stacktips.com/tutorials/design-patterns/template-method-design-pattern-in-java

public abstract class Pizza {
	public abstract void chooseBread();
	public abstract void addIngredients();

	public void heating() {
		System.out.println("Heating for 10 minutes!");
	}

	public void addTopinngs() {
		System.out.println("Adding Topinngs!");
	}

	public void addCheese() {
		System.out.println("Adding Cheese!");
	}

	// Template method
	public final void preparePizza() {
		chooseBread();
		addIngredients();
		heating();
		addCheese();
		addTopinngs();
	}
}
```

Where in subclasses, different flavors of pizzas making process, people can simply
override each individual step, leaving the assembling method (The template method)
alone, and the the core pizza making algorithm is always consistent.
