---
layout: "post"
title: "The Chaos Engineering and Chaos Monkey"
comments: true
---
<span style="background-color:rgba(51, 102, 204,1); color:white; padding:3px 4px;">Learn and Write</span>&nbsp;&nbsp;&nbsp;
<span style="background-color:rgba(204, 0, 0,1); color:white; padding:3px 4px;">Chaos Engineering</span>
<br/>
<br/>
![Chaos Monkey poster](/images/chaos-monkey-poster.jpeg)
(Image credit: <a href="https://medium.com/production-ready/chaos-monkey-for-fun-and-profit-87e2f343db31">Chaos Monkey for Fun and Profit</a>)
<br/>
<br/>

>Chaos Monkey randomly terminates virtual machine instances and containers that run inside of your production environment. Exposing engineers to failures more frequently incentivizes them to build resilient services. - Netflix Engineering

<br/>
The typical explanation of what Chaos Monkey is you can find online: it is a tool invented by Netflix in 2011 to test resilience of its IT infrastructure. There are many posts, wiki pages online talking about what it is and how it lines up with the trending DevOps model, so there is no need to add any redundancy here. But I would like introduce chaos engineering and chaos monkey in brief and add some of my understanding in bullet points and then concludes on the value it is bringing.
<br/>
<br/>
In brief, Chaos Monkey is a tool built to simulate random instance failures, which then more frequently exposes engineers to failures, thus encouraging them to build services of more resiliency. Chaos Engineering, as claimed, is "the discipline of experimenting on a distributed system in order to build confidence in the system's capability to withstand turbulent conditions in production."<br/>
<br/>
So, in a nutshell, Chaos Monkey is a proactive effort that brings down system parts by simulation in production environment to catch inherent problems in a distributed system. In addition, following are some bullet points that, I have summaried and I think, are essential in understanding Chaos Monkey:<br/>

1. <Strong>Chaos simulation should be conducted in production environment.</Strong>
    * Exercising on other non-production environment add uncertaintity if a similar problem exactly exists in production environment.
    * Problems are pre-assumed to exist left and right in non-production environment, new chaos are not necessary before existing problems are addressed and have itself turned into produciton ready.
2. <Strong>Starting point should be a healthy/steady point of a system.</Strong>
    * To have confidence identifying a problem and roo-cause it after, it makes more sense to control all other variables, including starting from a healthy point, which eliminates any other previous chaotic cause which might potentially be mixed in current analysis.
    * When not having a health starting point, a distributed system does not behave a normal fashion, which also contradicts the first rule.
3. <Strong>Automate the chaos in a continous manner.</Strong>
    * On top of considerations like labor-intensive, unsustainable for manual running, continous running also resonates the trend of continous integration.
    * Manual process, by design, is not portable when it comes to transferring work. It by nature demands more documentation, which in the end requires more time in comprehension of such doc.
4. <Strong>"Minimize the blast radius".</Strong>
    * For systems Netflix are running, millions of users will be impacted when there is no control of allowance for negative impact.
5. <Strong>Chaos Monkey is meant to bring up potential design problems inherent in a distributed system, not a testing of how it works.</Strong>
    * Testing of how system works and if it works should fall into integration tests.
6. <Strong>What Chaos Monkey brings is confidence we can incrementally build in our systems.</Strong>
    * By menas of "sumulations", we should be clear that Chaos Monkey is a software simulation as to real life outages, by which we identify and fix problems, which then builds up our confidence in system behavior when it is serving hundreds of customers if not millions.
    * There are many real-life outages in the field, which may not all be simultable or simulated, which also makes it a probability game.
7. <Strong>"Chaos Monkey tackles systematic uncertaintity".</Strong>

The value Chaos Monkey is bringing is straightforward: it pushes a distributed system to its extreme by simulate real-life systematic outages, which proactivly address any inhernt problem, which more importantly incentives engineers to design servies of more resiliency.<br/>
<br/>
To not make this post long and ugly to read, I'll stop here and encourage you to find other read online and check out Netflix's Chaos Monkey on github as well as its Chaos Monkey article. Happy Chaos Monkeying!<br/>
<br/>
-10000tb
<br/>
<br/>
References:<br/>
<a href="https://medium.com/production-ready/chaos-monkey-for-fun-and-profit-87e2f343db31">Chaos Monkey for Fun and Profit</a><br/>
<a href="http://principlesofchaos.org/">Principles of Chaos Engineering</a><br/>
<a href="https://en.wikipedia.org/wiki/Chaos_Monkey">Chaos Monkey on Wikipedia</a><br/>


{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}
