---
layout: "post"
title: "The recap you don't wanna miss for 2017 Jenkins World"
comments: true
categories: work
tags: [conference, write]
image: jenkins-world-banner.jpg
---
(Image credit @Cloudbees)  

I made a slack post within my current company(<a href="http://www.thoughtspot.com">Thoughtspot</a>) a few hours after the conference ended. Taking the fact of low number of responses following the post, I don't feel people's excitement around the new era of Jenkins. Moreover, I put a quite bit of time compiling it over the night. So instead of having it burried in coming channel chaos, I thought to myself: why not repost it here in my blog, and spread it through the holy globe of Internet.

(Cough cough, post starts)

It was really fun, and energetic two days in the city. Tiring as well, but the swag was good: I got a number of free t-shirts and stickers. So following are some highlights from my point of view for 2017 Jenkins World.

<strong>Sandwich</strong><br>
The sandwich at the conference was good, served on time. But just a lack of variety. Overall, 4.6 star. @CloudBees #DiversityOfSandwich

<strong>DevOptics</strong><br>
Cloudbees brought up and highlighted the concept of DevOptics. It was highlighted in opening keynote, and brought up again as one session from CloudBee's CTO. As to me, they are trying to orchestrate DevOps with DevOptics, which I am looking forward to. (At least the demo looked good)

- A series of products are coming up around it.
- It is really not just about Jenkins, but rather about the whole flow of DevOps. One example: there was one demo on how jira tickets flow through different modules as pipelines run, which was really cool.
- It gives you more about how different modules/parts perform in pipelines, which could help find bottlenecks in the development process.
- In addition, it has product coming up visualizing metrics on CI/CD pipelines.
- Those products won't be part of Jenkins. They will be separate commercial products. However, it will be able to talk to both open-sourced and commercial version of Jenkins.

<strong>Data Model of Jenkins</strong><br>
Data model? What is it? In a nutshell, it is a smarter Jenkins. Currently, Jenkins is onlly about users and pipelines/jobs. As referred to as data models of Jenkins, they are extending Jenkins to be aware of applications, and teams. So instead of just a whole bunch of standalone pipelines, you can pull together a group of pipelines under application, or team, which shows you that those pipelines aggregate to being this application. It looks pretty cool.

<strong>Pipeline as Code</strong><br>
We are already doing this within <a href="http://www.thoughtspot.com">Thoughtspot</a>, which is great!

- The trend is moving to source control pipeline scripts considering its multiple benefits.
- Create shared reusable library for pipelines.
- Declarative pipelines seem to have been adopted in many places (Mozilla, Github, etc.)

<strong>Disposable Jenkins</strong><br>
This is not a new idea, but rather a practice that worth adopting. The idea is to host Jenkins server through container, and have all server configurations automated through scripts. The main benefit is that if server dies, you can one click to bring up server without hours of pain to configure all those plugins, and server host.

Some key elements if you wanna go for disposable Jenkins:

- Have a jenkins server image.
- Automate plugins installations with scripts.
- Automate users sync with some LDAP store.
- Backup policy and separate persistent store for jenkins home volume.

<strong>Do things right</strong><br>
As from Cloudbees' consulting experience, there is a strong message that getting any organization, large or small, through cultural or technological barries to a good state of CI/CD is exponentally hard. However, doing things in a right way right, and following best practices can often achieve continuous delivery linearly. There was one session around best practices from CloudBees' CTO, which was pretty awesome in demonstrating the importance of following best practices.

<strong>Right Metrics</strong><br>
Having a right metrics to measure CI/CD. A bad examplge: using number of builds currently happening on server to measure the health of CI/CD. In addition, There is a product within DevOptics from CloudBees on metrics coming up.

<strong>Disasterproof Jenkins infrastructure</strong><br>
There was a session on this, and it was around AWS and container. The idea is to reduce the chance of pipeline failures from infrastructure issues.

<strong>Microservices with OpenShift from RedHat</strong><br>
RedHat introduced their openShift for microservices development (it supports K8s for container orchestration). It could be a great platform for microservices development.
![2017 Jenkins World](/assets/img/openshift-banner.jpg)

<strong>How to enable cross-component branch commits with dependency relations</strong><br>
Simples way is: don't allow dependency relations on commits across component branches. However, if there have to be relations, there is a way to make those commits go through qualifying pipelines smoothly.
![cross component commits with dependency](/assets/img/cross-component-commits-with-dependency.jpg)

- Build a topological dependency relations between commits.
- Do incremental build from those independent commits by following the topological relations.
- Use built artifact(s) for corresponding commits that depend on them.
- If there is circular dependent relations, break commits into subsets, do the same thing on subsets.
![topological dependency build](/assets/img/topological-incremental-build.jpg)

In the last, grand appreciation for my current employer's sponsorship of the conference ticket. 

-10000TB

{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}
