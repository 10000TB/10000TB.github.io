---
layout: "post"
title: "Trick of the Day: one line vim nmap to beautify bloody Json monster"
comments: true
---
<span style="background-color:rgba(51, 102, 204,1); color:white; padding:3px 4px;">Vim Trick</span>&nbsp;&nbsp;&nbsp;
<span style="background-color:rgba(204, 0, 0,1); color:white; padding:3px 4px;">Learn and Write</span>

<br/>

![vim trick for the day](/images/vim-trick-of-the-day-pic.png)

<br/>
<br/>

```
nmap =j :%!python -m json.tool<CR>
```

>Trick of the day to this line of magic.

I am recently taking over some work from a colleague, and I was experimenting his code base. Suddenly, I got a giant unformatted json dump. Not surprising, haha; As always, we need to beautify it to have it more readable.<br/>
<br/>
One quick way, I was taught by my previous lead, was beautifying with json module in interactive python shell. Namely: open python interactively, then do the trandformation and save beautified json.<br/>
<br/>
However, I was in vim, and it occur to me that given the powerfulness of vim and there are so many geeks around vim, there must be a trick that will just do in file json formatting. Then, I go the following result from search:<br/>
<br>
```
:%!python -m json.tool
```
<br/>
Essentially, logic of this trick under the hood is same as the native way I mentioned. However, it is way shorter in terms of steps needed.<br/>
<br/>
This solve the 99.9% of our immediate need to just format the json currenly in front you on screen. However, this line is hard to remember and type it out later when needed. It became a natural call to make a vim mapping for the line. Finally, I got the following:<br/>
<br/>

```
nmap =j :%!python -m json.tool<CR>
```

This is a great mapping suggestion from a comment of the original <a href="https://coderwall.com/p/faceag/format-json-in-vim">solution</a>. The main consideration of having `=j` is because `=` is known to vim world as formatting/indenting. It then become obvious and easy to remember to have `=j` dedicated for json beautifying!<br/>
<br/>
Enjoy and Cheers!
<br/>
<br/>
-10000tb

{% if page.comments %}
{% include comment-plugin.html %}
{% endif %}
