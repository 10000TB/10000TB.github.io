---
layout: "post"
title: "Groovy Compilation: AST and CPS from a Jenkins point of view"
comments: true
categories: work
tags: [work, write]
image: code-compiler-image-referenced.jpg
---

When you are trying to plugin in your pure Groovy library into your Jenkins CI pipelines, you often run 
into all kinds of compilation issues due to that Jenkins is leverging a customized CPS groovy compiler 
to parse pipeline steps instead of using native Groovy compiler. Even worse, those compilation issues 
printed from Jenkins console is not verbose enough for you figure out what actually went wrong. 
Here, I am writting down some frustrations I have encountered and also share some tips that I found 
helpful. To show a clear view and only demonstrate what I know, I am showing a list of bullet points.

1. [ Tip ] I found following way of writting a pure Groovy library works pretty well for me.
First, directory structure is as follow:

```
<GIT_ROOT>
|
|--src
    |--com
        |--<CompanyName>
                |
                |-------lib
                |        |---------deps
                |        |          |---<Self vendored dependent binary goes here>
                |        |
                |        |---------tests
                |        |          |---<Groovy Unit tests go here>
                |        |
                |        |---------test.sh
                |        |          |---<I usually have a bash to automate running of all tests>
                |        |
                |        <Groovy classes go in this level>
                |
                |-------<You might also have some groovy at this level>

```

I manage all tests in `src/com/<CompanyName>/lib/tests`. To run single tests, execute 
a command like `groovy -classpath src/com/<CompanyName>/lib/ src/com/<CompanyName>/lib/tests/SomeTest.groovy`. 
What worth mentioning here is that pure groovy is compiled by official Groovy compiler 
in AST. However, the groovy pipelines on Jenkns are compiled with a customized Groovy 
compiler wich compiles in CPS (continue passing style).

2. [ Frustration ] As Jenkins is using a customized Cps Groovy compiler, compiling our 
pure Groovy lib is likely hit a bug here and there, especially when Jenkins pipeline 
is still evolving and undergoing fast pace of development. In addition, there is too 
much freedoom when writting in pure groovy, which increases chances of hitting issue 
with integrating into Jenkins pipelines.

3. [ Frustration ] The `@NonCPS` annotation, as recommended by Cloudbees support for 
pure Groovy code block, doesn't solve problems magically. Take s step back, lets 
think about and discuss what is this annotation for ? The point I heard most is that 
adding this annotation enforces Jenkins to run annotated block of code in a one-off 
manner. To further clarify, let me begin with claiming that the key here is about 
serialization. Normal objects created in Jenkins pipelines must be serializable
so that Jenkins can serialize the state of the script in order to save it onto and restore
from disk. Use case like: Jenkins need to install new patches and ongoing jobs need to
be temporarily paused, and restarted after Jenkins upgrade. Then, lets go back to our original
point: with `@NonCPS` annotation for non-standard Jenkins groovy, it tells Jenkins that such
bocks are not serializable and need to be run till finish. Also, under this annotation, code
bloakcs will be compiled in AST. Ideally, that should help the situation when we want to plug
in a bunch of Groovy classes. However, from what I have tried, `@NonCPS` is not doing the magic
as claimed.


-10000TB
<br/>
<br/>
<br/>
References:<br/>
1, Image credit: code compiler image (https://blog.programminghub.io/blog/2017/02/28/offline-compiler-apps-solution/) 


{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}
