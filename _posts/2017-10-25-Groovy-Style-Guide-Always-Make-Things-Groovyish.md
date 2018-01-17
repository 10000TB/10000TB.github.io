---
layout: "post"
title: "That Groovy Style Guide - Always make things more Groovyish"
comments: true
---

1.<Strong> Use idiomatic syntax </Strong>  
I have compiled a list of idiomatic Groovy syntax down below. Check them out.
  
2.<Strong> Take advantage of features provided by Groovy </Strong>
  
  
  
![Groovy image](/images/groovy-logo-banner.jpg)  
  
  

I would recommend this style because of following two reasons:

1. Whenever you write Groovy, you'd write better real Groovy and take advantage of what this language is capable of.
 Whenever you touched a file with a `groovy` suffix, you already admit that you are writing Groovy! Although 
 Groovy is created like java as an object oriented language for Java Platform, it is also a dynamic 
 language, you now have access to a lot features that you like from python, Perl, SmallTalk, and the like. 
 One great featuer is dynamic typing: you can use `def`to define a variable of any type, which give me a 
 lot of flexibility when I want a clean and concise code/class structure. Therefore, take advantage of what Groovy
 is capable of and write idiomatic Groovy.

2. Avoid Confusion. Groovy is often compared with Java, so it is even more crucial that we need to write
Groovyish things to avoid confusion in production environment, expecially when you are using both of them in
production.

Down below, I have compiled a list of Groovy features that are not available in Java, and I have written some code
snippets if applicable:  
  
I. <Strong>Static Typing and Dynamic Typing</Strong>  
* Use mixed dynamic and statically typed methods to take advantage of benefits from them
* No need to do type casting before using a method of an object

Groovy, as claimed, is and will always be a dynamic language. However, since Groovy 2.0, Groovy added support
for static type checking. Great news for those who work extensively with Java and want a piece of embedded Groovy
in their Java but still be able to catch error(s) during compilation.  
**Static Typing**  
Example 1: No matching method  
Groovy provides a annotation `TypeChecked` to notify groovy compiler to do static checking:
``` 
import groovy.transform.TypeChecked

void printHaha() { println "David is handsome" }

@TypeChecked
void test(){
    // Lets say you actidently mistyped method name
    // and you ended up with calling a non-exist method
    // you will be prompted a compilation error during compilation.
    
    // Compilation error: cannot find matching method
    printHahaaaaa()
}
```
There many more errors that can be caught with `TypeChecked` annotation. Below is a list that I compiled:  
&nbsp;&nbsp;&nbsp;&nbsp;a) **assignment type check**.  like `int a = "I am a string"` will be complained: `Cannot assign string to a variable of int`  
&nbsp;&nbsp;&nbsp;&nbsp;b) **return type**. like below: `String test(){ return 1 }` will be complained with `Cannot return type of integer on method of return type String`.  
In addition: `return` from `if/else`, `try/catch`, `switch/case` block will also be checked at comilation time.  
For example:
```
import groovy.transform.TypeChecked

@TypeChecked
int test(name){
    switch(name){
        case 'David':
            return 112
        case 'Maxim':
            return 'Maxim'
    }
}
```
will complain return type doesn't match the return type of the method. To be exact:
``` 
org.codehaus.groovy.control.MultipleCompilationErrorsException: startup failed:
test.groovy: 9: [Static type checking] - Cannot return value of type java.lang.String on method returning type int
 @ line 9, column 20.
               return 'Maxim'
                      ^

1 error
```
One thing worth pointing out is that this is during comilation time, and all cases blocks will be checked!

&nbsp;&nbsp;&nbsp;&nbsp;**Note:** However, common type conversions that Groovy supports are still allowed and Groocy compiler will not complain about it.  
For example, using the same example like above but make a slight modification:
```
import groovy.transform.TypeChecked

@TypeChecked
String test(name){ // The only change is: I changed return type to String
    switch(name){
        case 'David':
            return 112
        case 'Maxim':
            return 'Maxim'
    }
}
```
This will compile without any problem. So what happens is that for `case 'David'`, the return `112` will be automatically
converted to string: `"112"`.  
Then you might be wondering what common conversion include ?  
I got you :) . Common conversion like:  
String -> boolean  
object -> String ( object resolved to String by toString() )
var -> class  
are all supported. To summarize, I quote Geoocy release note:  
 >For method signatures returning String, boolean or Class, Groovy converts return values to these types automatically:

To include everything release, I shall also mention that `type inference` is also supported in Groovy. For example:  
```
import groovy.transform.TypeChecked

@TypeChecked
String test() {
    def company = "ThoughtSpot"
    
    println "I work at: ${company.toUpperCase()}"
}

test()
```
Groovy will be smart enough to know company is of type String and use GDK methods `toUpperCase()`.

Although static typing bring the convenience of reporting errors at compile time, it
prevents from using many dynamic features provided by Groovy. In order to take advantage of both, we can use
mixed dynamic and statically typed methods. `TypedChecked` and `@TypeChecked(TypeCheckingMode.SKIP)` can be used
to add and exclude a scope for static type checking.

One more example of type inference:
```
 import groovy.transform.TypeChecked

 @TypeChecked
 void test(Object val) {
     if (val instanceof String) {
         // Type checker know in this scope,
         // that val is of type String.
         println val.toUpperCase()
         // As groovy can do type reference,
         // you dont need to do ((String)val).toUpperCase()
         // as in Java.
     }
 }

 test("no need to case type")
```

Some cases in terms of type inference when Groovy will throw errors:  
1. When Groovy is not able to decide the type at compile time.  
(TODO) Continue from "Flow Typing"
  
  
II. **Operators overriding to write concise code.**
As operators in Groovy are implemented by methods, we can override those methods in
our own methods so that we can have a concise code.  
For example:  
```
 class Weight {
       def val

       Weight plus(Weight w) {
           new Weight(val:this.val + w.val)
       }

       int hashCode() {
           val.hashCode()
       }

       String toString() {
           val
       }
 }

 def w1 = new Weight(val:12)
 def w2 = new Weight(val:13)

 println w1+w2
```
This will produce: `25`

In addition, below is all operators and their corresponding methods in Groovy:  

| Operator	| Method      |
|-----------|:-------------:|
| a + b     | a.plus(b)   |
|a - b      | a.minus(b)  |
|a * b      |a.multiply(b)|
|a ** b     |a.power(b)   |
|a / b      |a.div(b)     |
|a % b      |a.mod(b)     |
|a l b      |a.or(b)      |
|a & b      |a.and(b)     |
|a ^ b      |a.xor(b)     |
|a or a     |a.next()     |
|a-- or --a |a.previous() |
|a[b]       |a.getAt(b)   |
|a[b] = c   |a.putAt(b, c)|
|a << b     |a.leftShift(b)|
|a >> b     |a.rightShift(b)|
|a >>> b    |a.rightShiftUnsigned(b)|
|switch(a) { case(b) : }|b.isCase(a)|
|~a         | a.negate()  |
|-a         |a.negative() |
|+a         |a.positive() |
|a == b     |a.equals(b)  |
|a != b     |! a.equals(b)|
|a <⇒ b     |a.compareTo(b)|
|a > b      |a.compareTo(b) > 0|
|a >= b     |a.compareTo(b) >= 0|
|a < b      |a.compareTo(b) < 0|
|a ⇐ b      |a.compareTo(b) ⇐ 0|
|as as type |a.asType(typeClass)|


III. **Use native list in Groovy**  
Here is an example:  
```
 def list = []
 println list.class
 // This return: java.util.ArrayList
 // So by default, with def, ArrayList is created.
 def list1 = ["item1", 2, 3.14]
 println list1
 // Values can be of different types.
 def list2 = ["item1", 2] as LinkedList
 println list2.class
 def list3 = ["item1", 2] as Vector
 println list3.class
 // Specify the list implementation.
 println list.size()
 // Size.
 list1 << "item2"
 list1.add("item3")
 println list1
 // Add a new item
```
It will produce following output:  
```
class java.util.ArrayList
[item1, 2, 3.14]
class java.util.LinkedList
class java.util.Vector
0
[item1, 2, 3.14, item2, item3]
```
IV. **Automatic check for null pointer**  
The safe navigation operator `?` can be used to check null pointer automatically.
For example:  
```
if (obj?.someMethod()){
    // do some thing.
}
```


(TODO:)Add more.


References:  
[Groovy (Programming Language) ](https://en.wikipedia.org/wiki/Groovy_(programming_language))  
[Apache Groovy Style Guide](http://groovy-lang.org/style-guide.html)  
[Release Notes for Groovy 2.0](http://groovy-lang.org/releasenotes/groovy-2.0.html#Groovy20releasenotes-Astaticthemeforadynamiclanguage)  
[Groovy Examples](http://gr8labs.org/getting-groovy/)  

{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}