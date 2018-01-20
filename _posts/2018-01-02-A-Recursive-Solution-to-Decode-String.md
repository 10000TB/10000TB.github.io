---
layout: "post"
title: "A recursive solution to decode strings"
comments: true
categories: algo
tags: [algo]
image: matric-decoded-image.jpeg
---

The problem:<br/>
<br/>

```
Given an encoded string, return it's decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

Examples:

s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```
<br/>
<br/>
First and foremost. the problem could have been extremely easier if `nested encoding structure` is not allowed. For example, the encoded string will only be like number[pure string], For example: 3[abds]4[sdsd]sdf, or 3[sdf]121[asddfs]asd3[asd]asd, etc. If so, the solution is very easy, we simply parse the number first and use `substring()` to get the inner string and make specific times duplicate string into final return string, and that is it.<br/>
<br/>
But the reality is that the inner string can be another layer of encoded string, which is number[ number[string] ], or even deeper. However, if we only take the top level encoding into consideration, no matter how deeper the inner coding does the string have, the overall structure will always look like this:

```
NUM[....]NUM[....]NUM[....]
```

(of course there might be some pure characters exist between each encoding unit(`number[...]`)), so generally, and top level encoded string will like this:

```
absadNUM[....]NUM[....]adaNUM[....]
```

(above is just an example, actual characters may vary and they might exist or not exist between encoding unit)
<br/>
<br/>
The solution will be enormously similar, we still parse number and the inner string, instead of treating the inner string as pure string, we call `decodeString()` again  on this inner string, because it can also be a encoded string.( YES, it can also be pure character string, but that way, inner call will just return the pure string, no big deal )
<br/>
<br/>
So here comes the solution:<br/>

```
public String decodeString(String s) {
    StringBuilder sb = new StringBuilder();
    int i=0, len=s.length(), start=-1 ; //i indicate which character is at;
                                                   //start indicate the index of the '[' in a encoding unit
    Stack<Character> st = new Stack<Character>(); //typical way to parse [...] structure
    if(s==null ||s.length()==0) return s;//check null or  empty  string, just return it if so
    
    while(i<len){
        if( s.charAt(i)>'0' && s.charAt(i)<='9' )
        {
            int nextLB = s.indexOf('[',i);  //get the index of next '[' after this number
            String cnt_string = s.substring(i,nextLB);//the number could be multiple digits
            int cnt = Integer.parseInt(cnt_string);//parse the number from string
            i = nextLB+1;//move pointer to next character after this '['
            st.push('[');
            start = i;
          while( i<len && st.size()>0 ){// here we are finding index of closing ']' of a top unit
                if(s.charAt(i)=='[') st.push('[');
                if(s.charAt(i)==']') st.pop();
                i++;
            }
            String inner_decoded = decodeString(s.substring(start,i-1));
            for(int j=0; j<cnt; j++) sb.append(inner_decoded);
            
        }else{
            sb.append(s.charAt(i));
            i++;
        }
    }
    return sb.toString();
}
```
<br/>
<br/>
<br/>
Rferences:<br/>
1. Image credit: matrix decoded image (http://www.theeventchronicle.com/study/the-matrix-decoded-decoding-the-occult-messages-in-the-matrix/#)


{% if page.comments %} 
{% include comment-plugin.html %}
{% endif %}
