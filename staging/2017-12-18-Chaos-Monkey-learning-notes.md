---
layout: "post"
title: "Should you bring Chaos Monkey into your organization?"
---

As I step on to taking over our(<a href="http://www.thoughtSpot.com">ThoughtSpot</a>) internal Chaos Monkey work, I started to revisit some public Chaos Monkey articles, and open source projects. For those who are not familiar with Chaos monkey, it is usually a resiliency tool used to inject random system failures, which potentially and ideally would expose engineers to problems in system to problems by those failures. It then incentives engineers to build more resilient services.<br/>
<br/>
We have built our own Chaos Monkey service at ThoughtSpot, and it has brought in many benefits and is very promising. I would like to use this post to share what we have built
