---
layout: "post"
title: "A curated list of those Vim How-Tos we search frequently in our daily coding"
comments: true
categories: vim
tags: [vim, blogseries, dummynotes]
image: "blog-series-vim/blog-series-vim.png"
---

Vim has been my editor for day to day work for a while, and often times I find myself questing same vim How-To on stackoverflow and websites as such. To help manage a list of my frequent used Vim tips/commands, and potentially be useful to you, I am curating this list of vim How-Tos.  
  
- [x] **How to search within visually selected text in vim ?**  
  For many reasons/occasions, we need to search within a scope, for example within visually selected text in vim. A simple way:  
  
  1. _Visually select text. (With v, V or Control-V)_
  2. _Press ':' (colon)_
     > Now you should see prompt as `:'<,'>`
  3. _Following that, type in seach or seach replace command, then the search/search and replace will only apply within the selected scope_

  Demo:  
  Say you have a list in python and you forgot to quote items, and you can take advantage of this vim tip instead of adding quotes for each of them mannually.  
  ![selected-search-with-demo](/assets/img/blog-series-vim/selected-search-replace.gif)

- [x] **How to cancel search hilight ?**  
  There are times when you have unwanted search highlight, and you simplely want to cancel the highlight. `:noh` command exactly do the trick:  

  1. Simply type `:` (colon) and follow with `noh`

  Demo:
  ![cancel-search-highlight](/assets/img/blog-series-vim/cancel-search-highlight.gif)


