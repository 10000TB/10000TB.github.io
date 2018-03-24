---
layout: "post"
title: "A curated list of those Vim How-Tos we search frequently in our daily coding"
comments: true
categories: vim
tags: [vim, blogseries, dummynotes]
image: "blog-series-vim/blog-series-vim.png"
---

Vim has been my editor for day to day work for a while, and often times I find myself questing same vim How-To on stackoverflow and websites as such. To help manage a list of my frequent used Vim tips/commands, and potentially be useful to you, I am curating this list of vim How-Tos.  

- [x] visual selection in vim ?  
  `Ctl-v` - visual block.
  `v` - select chars.
  `V` - select current line.

- [x] Shortcuts to move cursor in vim ?  
  `gg` - to move to start of file.
   `G` - to move to end of file.
  `''` - to move to last active point.
  `0` - to move to start of current line.
  `$` - to move to end of current line.

- [x] How to run command within vim ?  
  Syntax: `:%![cmd]`:  
  Here `%` represent that we pass in whole buffer of vim into command for filtering. We can also specify a range instead of passing the whole buffer. For example: `:2,5![cmd]`, `:1,100![cmd]`
  A list of examples of running commands within vim:  
  ```
  # Do word count.
  :%!wc
  
  # 
  ```
  For buffer to be used, when a visual selection is active, that will automatically be used as buffer input.

  For reference:  
  * `%!`: pass current buffer.
  * `4,!`: line 4 through end of file.

- [x] How to beautify ugly json within vim?  
  With command `python -m json.tool`:  
  ```vim
  :%!python -m json.tool
  ```
  This trys to beautify the whole file.  
  If there is a need to only beautify a range of lines, use following instead:  
  ```
  :[start],[end]!python -m json.tool
  ```
  To make it even more convenient for your daily work, `nmap` this command to some short cut.  
  Following nmap maps the first command to `=j` shortcut:  
  ```
  nmap =j :%!python -m json.tool<CR>
  ```
  
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


