---
layout: "post"
title: "That Groovy Style Guide - Semicolons"
comments: true
---

That Groovy Style Guide - Semicolons

1. <Strong> No semicolons </Strong>  
* it is more idiomatic to have no simicolons in Groovy
  
  
  
![Semicolons dianosours](/images/semicolons-banner.png)
  
I would strongly recommend this syle because of two reasons as stated below.

First of all, Apache says `No semicolons` in their Groovy style guide. As they pointed out, 
it is more idiomatic to have no semicolons in yout Groovy code.

In fact, Groovy supports 99% of Java syntax, and it will work in most 
cases if you copy a snippet of java and paste it into a Groovy context. However, it is not 100% supported, right?  
Since java syntax is not 100% supported, lets not mess Groovy up with Java. In this case, lets abandon semicolons.
Therefore, another weaker argument of not having semicolons in Groovy: Java syntax is not 100% supported, so use no semicolons
to clearly declare what is written is pure Groovy.

As always, comment below if you have a different view, and lates and best style will be updated to this post.

-10000TB

References:  
1. [Apache Groocy Style Guide](http://groovy-lang.org/style-guide.html)

{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}