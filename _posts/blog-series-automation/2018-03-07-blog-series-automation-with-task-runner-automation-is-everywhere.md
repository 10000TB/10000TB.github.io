---
layout: "post"
title: "Tasks runner forges the concept of automation from a more generalized perspective"
comments: true
categories: automation
tags: [blogseries, automation, learn&share]
image: "blog-series-automation/blog-series-automation-banner.png" 
---
![grunt-vs-gulp](/assets/img/blog-series-automation/grunt-vs-gulp-banner.jpeg)
>(Image(Grunt vs Gulp) credit: https://medium.com/@preslavrachev/gulp-vs-grunt-why-one-why-the-other-f5d3b398edc4)
  
I was checking a system test failure at work. Along the way, I found out that all existing scenarios rely on Grunt, a javascript task runner. Speaking of "task runner", I was working with python Invoke, a python (2.7 & 3.4+) task execution tool & library. Task runner, I think and as they themselves claimed, automates things and make people's life easier. In addition, my work mainly deals with automation recently, so getting to learn more on task runner, understand the inner
beauty of it, and explore wider range of use cases become a natural thing for me.  
  
I could be wrong, but from what I know/see/hear, people speak of automated test scenarios when talking about automaion most of the time. Lets call this a narrow perspective.  
  
In a broader sense, I think, software itself, not just those testing scenarios for quality assurance, is an automation. Software could be general customer-facing product, internal dev tools, even some utility scripts that just do one thing at a time, etc. Given the process is standard, universal at some level, repeatable, it is then automatable, and automation can be called as such.  
  
Grunt can help register tasks, which you can define what to do in the domain of javascript. With the plugin ecosystem of grunt, frequently and commonly used task scenarios are written/shared in the form of plugin, which saves the hassle of doing it yourself.  
  
A simple demo of grunt task definitoin can be like following:   
```javascript
 ... {    
     ... // ignore configs here
     
     grunt.registerTask('task2', ['step1', ['step2', 'step3']])

     ... //other tasks
 }
```
  
Later on, you can simply issue a grunt command `grunt task2`, and it will execute all steps upon the firing of the command.  
  
If we look at this logic here, what to automate is identified by task name, and for each task, steps are what actually to do. Similary, in a customer-facing software, many features are identified by labels/buttons, or some sort of interactive item/identifier. Upon click/interaction, you get an automated outcome. Under the hood, it does nothing different but repeating same steps to deliver you a result(and result can be different based on different input). The difference is that, given the complexity of scenarios in real world and it is
customer-facing, the real steps logic will be more complex, and task notion/representation will be more complex. But in the end, once the software is delivered to end users, the execution logic is pretty fixed. Therefore, it is safe to say, general software is a form of automation.  
> By 'fixed', I dont mean "won't change", but rather: given an input, we would usually have an expected result.
  
Invoke, in the domain of python as mentioned at the beginning, serves similar purpose. Imagine that you are working on building a Restful API services and you are using swagger. In addition, you use docker to containerize your apps/services. Everytime, you make a modification, you want to test out the API response at the exact endpoint in no time.  
  
To achieve that, what do you think that need to be set up everytime ?  
  
Building new docker images for services containers, and of course start them afterwards; Environment set up for those services servers; Start you swagger server; Finally, in middle when you build your services container images, you might need to run some unittests, which you might end up forgetting to run in manual setup, which might end up giving you a server error.  
  
Luckily, with Invoke, you simply define all those tasks in order and steps ONCE, and you get the convenience of starting everything with one simple command.  
  
```python
from Invoke import task

@task
def build(args...):
    .... // building logic.

@task
def run_tests(args...):
    ...  // schedule tests to run.

@task
def setup_database(args...):
    ...  // set up a testing db, and load testing fake data.

@task
def start_container_and_setup_env(args...):
    ...  // start all services container, and do necessary env set up.

@task
def start_swagger(args...):
    ...  // start logic.

...  // more tasks.

```
> This is a simple example for proof of concept. With the power of Invoke, you can write/structure/manage you tasks in a more structured and modular way.
  
Then you can simply run any task by its name, for example: `Invoke build`, `Invoke run_tests`, etc. In addition, you can customize the order and combination of tasks to fit your needs.
  
Task runners, as exampled above, is solving the problem of repeatition by automate them and make them into reusable template and can be reused with firing of a simple command. Although, it is not comparable to real software automation, the concept, however, remains the same.

