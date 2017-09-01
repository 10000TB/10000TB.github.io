---
layout: "post"
title: "The Recap you don't wanna miss for 2017 Jenkins World"
---
![2017 Jenkins World](/images/jenkins-world-banner.jpg)
(Image credit @Cloudbees)  

I made a slack post within my current company(<a href="http://www.thoughtspot.com">Thoughtspot</a>) a few hours after the conference ended. Taking the fact of low number of responses following the post, I don't feel people's excitement around the new era of Jenkins. Morever, I put a quite bit of time compiling it over the night. So instaed of having it burried in coming channel chaos, I thought to myself: why not repost it here in my blog, and spread it through the holy globe of Internet.

(Cough cough, post starts)

It was really fun, and energetic two days in the city. Tiring as well , but the swag was good: I got a number of free t-shirts and stickers. So following are some highlights from my point of view for 2017 Jenkins World.

<strong>Sandwich</strong><br>
The sandwich at the conference was good, served on time. But just a lack of variety. Overall, 4.6 star @CloudBees #DiversityOfSandwich.

<strong>DevOptics</strong><br>
Cloudbees brought up and highlighted the concept of DevOptics. It was highlighted in opening keynote, and brought up again as one session from CloudBee's CTO. As to me, they are trying to orchestrate DevOps with DevOptics, which I am looking forward to. (At least the demo looks good, )

- A series of products are coming up around it.
- It is really not just about Jenkins, but rather about the whole flow of DevOps. One example: there was one demo on how jira tickets flow through different modules as pipelines run, which was really cool.
- It gives you more about how different modules/parts perform in pipelines, which could help find bottlenecks in the development process.
- In addition, it has product coming up visualizing metrics on CI/CD pipelines.
- Those products won't be part of Jenkins. They will be separate commercial products. However, it will be able to talk to both open-sourced and commercial version of Jenkins.

<strong>Data Model of Jenkins</strong><br>
data model ? What is it ? in a nutshell, it is a smarter Jenkins. Currently, Jenkins is onlly about users and pipelines/jobs. As referred as data models of Jenkins , they are extending Jenkins to be aware of applications, and teams. So instead of just a whole bunch of standalone pipelines, you can pull together a group of pipelines under application, or team, which show you that those pipelines aggregate to being this application. It looks pretty cool.

<strong>Pipeline as Code</strong><br>
We already doing this within Thoughtspot, which is great!

- The trend is moving to source control pipeline considering its multiple benefits.
- Creating shared reusable library.
- Declarative pipelines seems to have been adopted in many places (Mozilla, Github)

<strong>Disposable Jenkins</strong><br>
This is not a new idea, but rather a practice that worth adopting. The idea is that hosting Jenkins server through container, and have all server configurations automatd through scripts. The main benefit is that once server dies, you can one click to bring up server without hours of pain to configure all those plugins.

- Automate plugins installations with scripts.
- Automate users sync with some LDAP store.
- backup policy and separate persisten store for jenkins home volume.

<strong>Do things right</strong><br>
As from Cloudbees' consulting experience, there is a strong message that getting any organization, large or small, through cultural or technological barries to a good state of CI/CD is exponentally hard. However, doings things right, and following best practices can often achieve that linearly. There was one session around best practices from CloudBees' CTO, which was pretty cool in demonstrating the importance of following best practices.

<strong>Right Metrics</strong><br>
Having a right metrics to measure CI/CD. A bad examplge: using number of builds currently happening on server to measure the goodness of CI/CD. There is a product withing DevOptics from CloudBees on metrics coming up.

<strong>Disasterproof Jenkins infrastructure</strong><br>
There was an session on this, and it was around AWS and container. The idea is to reduce the chance of pipelines failure from infrastructure issue.

<strong>Microservices with OpenShift from RedHat</strong><br>
RedHat introduced their openShift for microservices development (it supports K8s for container orchestration). It could be a great platform for microservices development.

<strong>How to enable cross-component branch commits with dependency relations</strong><br>
Simples way is dont allow dependency relations on commits across component branches . However, if there have to be relations, there is a way to make those commits go through qualifying pipelines smoothly.

- Build a topological dependency relations between commits
- Do incremental build from those independent commits by following the topological relations
- Use built artifacts for dependent commits
- If there is circular dependent relations, break commits into subsets, do the same thing.

In the last, grand appreciation for my current employer's sponsorship of the conference ticket. 

-10000TB
