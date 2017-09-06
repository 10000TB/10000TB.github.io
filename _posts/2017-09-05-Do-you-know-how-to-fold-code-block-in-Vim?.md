---
layout: "post"
title: "A-joke-on-Google-and-a-trick-on-Vim-for-today"
---
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

Without further due, let me clarify how folding code block can help. It is easy, you see the end 


-10000TB
