---
layout: "post"
title: "That Groovy Style Guide - Constants"
comments: true
---

That Groovy Style Guide - Constants
  
  
1.<Strong> Do things in object oriented way</Strong>  
Remember that it is still a language of Object Orientation  

2.<Strong> No simi-colon</Strong>  
Lets do everything in a Groovy way  

3.<Strong> Define constants in a separate class for external access</Strong>  

4.<Strong> Define a class for constant type instead of primitive types when constant need to have other attributes </Strong>  
Like: deploymentEnvironment: dev, production, testing.  
  
  
  
![esign joke: design gan with dev](/images/design-joke-banner.jpg) 
  
Recently I have been working on finding a way to write a portale Groovy common 
library for Jenkins pipelines at <a href="http://www.thoughtspot.com">ThoughtSpot</a>. 
By portable, I mean writing common functions/utilities that is pure Groovy without using 
any Jenkins plugin step. It is clear that we will go for this kind of portable Groovy common 
lib instead of having any dependency Jenkins Plugins step, which is only useful on Jenkins server. 
After searching/referencing here and there, I finally had a working version of a pure Groovy lib. 
But, a problem rose in code review phase: what code style should we adopt for native Groovy code 
in production ? It is crucial that we should write code in best practice so that it is maintainable 
in the long run. One heated discussion we had for my first version deliverable is about constants. 
We went for Groovy official style guide, and it was helpful, but it doesn't include everything as 
expected. Specifiaclly,  we didn't find a solid official guide on how to structure global constants 
for reference, internal discussions were subjectively skewed, like we should follow what we did in 
python, etc. After I spent some time investigating on it, I found many good articles demonstrating a 
good way to manage constants. Together with my own understanding, I present you: That Groovy Style Guide - Constants as listed above. 
('That' here to differentiate this from any official documentation)  
  
Lets take an look at a few examples and see how I recommend the style above:
0. You can always define constants local to a file/class and only use them in that file. Skip that here.

1. A basic example:

```
class Constant {
    public static final String WORKSPACE = "/home/user/workspace"
    public static final String DOCKER = "docker"

}
```
This basic example can help you manage all commonly used primitive constants in one place, but giving access to all places whereever it is imported.

2. When you need to manage constants in a more complex cases: for different deployment mode(dev, prod, testing), fir different release version, have a constants class like above is no longer manageable, extendable. Here is an example that can address all those needs and provide many other benefits in design:

```
class Constant {
    public static final Constant WORKSPACE = new Constant("1.0", "/home/user/workspace", "production")
    public static final Constant WORKSPACE = new Constant("2.0", "docker", "testing")

    def version
    def value
    def mode
    private Constant(version, value, mode) {
        this.version = value
        this.value = value
        this.mode = mode
    }

    public toString() {
        return this.value
    }

    // Define helper methods around versions and mode.
}
```
Here with Constant itself defined as a type, we are able to keep track to: 1) Since which version of lib, such constant is available. 2) Which mode is one constant applied to. It is not possible with simply using primitive types. In addition, using multiple classes/files for different versions and modes my make that happen, but it is neat and manageable in the long run since in competitive markets, products evolve fast, which makes versions and modes change fast.

There are many arguments on above approach is bad. Here, in order to show above design is relatively good, I am addressing all those arguments by list what arguments are and providing my defending opinions.

1. Defining an object instead of primitives consume more space!
Not really. Although they are defined as objects, they are static, and only initialized once, and shared among all places. So, it won't consume much space because of unnecessary initializations. However, when number of constants grow, it will consume a lot of unecessary space expecially, not every constant is used every time. To address that, lazy initialization can be used when you have a large number of constants in a class. Furthermore, if there is multi-threading, use "double-check" idiom for synchronized inittialization.

2. It is not straight-forward to use object type as constant value!
Not really! Remember it is still an language of object orientation. whenever you need to use an object type, use it. It is true that constant is meant to store values and provide easy access, but dont't forget that in a lot of cases, you need to know more info about the constant before you can use it, and object type address that need.

3. Comment below if you find any other concern that is not listed.

-10000TB


References:
[Java Tip 67: Lazy instantiation](https://www.javaworld.com/article/2077568/learn-java/java-tip-67--lazy-instantiation.html)  
[Groovy Style Guide](http://groovy-lang.org/style-guide.html)  
[Java Object Size Calculations in 64-bit](http://btoddb-java-sizing.blogspot.com/)  
[Groovy Object orientation](http://groovy-lang.org/objectorientation.html)  
[Create enumerated constants in Java](https://www.javaworld.com/article/2076970/core-java/create-enumerated-constants-in-java.html)  
[Use constant types for safer and cleaner code](https://www.javaworld.com/article/2076481/learn-java/use-constant-types-for-safer-and-cleaner-code.html)  


{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}
