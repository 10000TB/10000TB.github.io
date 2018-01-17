---
layout: "post"
title: "HaDoop MapReduce to detect the author of a mysterious doc based on 1.2GB corpus with Cosine Similarity"
---
  


1 <Strong>What is this about?</Strong>

Given a 1.2GB corpus, where there are thousands of authors and their novels/articles, and a mysterious novel/article, find out the top ten authors who might wrote this mysterious novel/article from the corpus athors.

2 <Strong>Implementation</Strong>

 Summary of implementation
  There are two steps: first, we calculate the author attributes of the 1.2G corpus, which will generate a author attributes file of about 900MB. Second, we get the mysterious doc and cauculate this mysterious author's writing style(attribute), and calculate the cosine similarty between this author and each of authors in the corpus, and finally output top ten authors with top ten cosine similarity values

![Summary of Implementation](/images/MysteriousAuthorFnder.png)


<p style="font-size:10px;">
<span style="color:red;">This image was made by me purely for illustrating the project, all rights reserved ©Xuehao(David)Hu</span><br/>
*M and R: represent mapper and reducer in each step respectively; *Unigram: each word in [AuthorName]'s document;<br/>
*TF: Term Frequency = {how many times this Unigram occur in [AuthorName]'s document}/ {how many times the most frequent Unigram occur in [authorName]'s docuemnt } <br/>
*IDF: Inverse Document Frequency = log(N/n)/log2, where N is total number of authors in the corpus, and n is how many authors in the corpus are using current unigram at least once in their novel/article 
</p>
-D
