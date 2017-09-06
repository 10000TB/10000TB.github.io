---
layout: "post"
title: "A Joke on Google and a trick on vim for today"
---
![Exitting Vim finally](/images/exit-vim.png)

(How to exit vim. Image credit: the holy globe of Internet)

<strong>Warning</strong>: If you don't use Vim/Vi, don't bother reading any more letter.

Before getting started with text folding in Vim, I would like to tell a joke. :). The joke starts with a question:
>"Is Google male or female?". 

Wdyt? I will reveal the punchline somewhere in this post.

So I really adopted Vim since I joined my current organization, and I have been 
constantly and tirelessly searching and practicing new vim trick on a daily basis since then. 
Today(A.K.A Sept 5, 2017), when I was working on a groovy pipeline, and I was trying to remove 
an intermediate function block, I ran into a problem. The problem was that the block scope was too long
 so that I cannot locate the closing curly bracket. For example, I have following snippet:
```
  node("some-slave-label") {
    // Pipeline logic starts.                        
    //       
    //  ---------------------------
    //                            |                        
    //   Imagine that the height of this scope is that of my monitor
    //                            |
    //  ---------------------------
    //
    // Pipeline logic ends
   }
```
As you can see, you cannot have both starting and closing curly bracket showing up on screen at the same time.
That make you not able to see the highlight on the other curly bracket when your cursor is on one of the them.
You may argue that if the code is properly indented, it is easy to locate the corresponding curly bracket.It is 
easy only when the block is outmost, and the code is properly indented. However, the code here is not properly indented
as most things are still experimental, and the code block to remove is not outmost, but intermediate. So the same problem
unsolved. If you don't have a visual, here is an example to demonstrate the diffculty:
```

 timeout(some-time-out-value){
  ...
   node("some-slave-label") {
     // Pipeline logic starts.                        
     //       
     //
     //  ---------------------------
     //                            |                        
     //   Imagine that the height of this scope is that of my monitor
     //                            |
     //  ---------------------------
     //
       }
      ...
      }
     // Pipeline logic ends
     }
    }
   ...
 }
```

Without further due, let me clarify how folding code block can help. It is easy, you see the end when you fold the block. 
Then you can delete the intermediate function block easily. But then, how do I fold this intermediate block?

After searching, many folding techniques can be achieved natively in Vim, like folding by syntax, indent, folding and unfolding
 upon current cursor, etc. But the exact thing I wanted to achieve didn't come into my sight as handy. Then I gave up.

Lets take a pause and go back to the puzzle threw at you at the beginning. "Is Google male ot female?", "Female! because it 
always cant wait to reveal suggestations before you could finish a sentence."

What do you think? 

This is a good one without a second thought. Frankly, I laughed. you probably did as well. When you rethink about it, it is still
probably ok. However, if you think more about it, you would agree that people can easily argue that this is not a good one, as this
is against women. It can then become gender inequality, bla bla...
 
The point: it is because I said it was a joke at the beginning, and you easily fall into the hole set up by me. You then enjoyed the 
punchline as I saied there will be punchline coming up at the beginning. Therefore, you probably didn't doubt the revealed punchline
 and laughed thereafter.

Similarly, I fell into trying to get the folding work, and ignored other simple ways to achieve what I wanted. Finally, the way it worked 
is: start from the starting curely bracket, and use `%` in normal mode, which lead you the closing bracket. Then you delete the closing 
bracket. Then you do `""`, which help you go back to last cursor point. Then you can delete the opening part of the intermediate function.

I would not summarize anything here, and simply ask What do you think?

-10000TB
