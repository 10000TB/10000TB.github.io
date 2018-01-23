---
layout: "post"
title: "Python's Built-in function Reduce() as a JSON walker"
comments: true
categories: python
tags: [blogseries, python, dummynotes]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---


I was doing a code review, and a use of python's built-in `Reduce()` came to my attention. For same problem, I would definitely go with a simple recursive soluton, which is straightforward and easy to maintain(a.k.a understand). But the author of the code used python's built-in function `Reduce()`. I didn't get it at first, but managed to grasp it with some hassile of googling.  
  
Let me define the problem first:  

```
Given a nested JSON, retrieve the value from the JSON by a list of ordered keys.  

For example:
Given a json(lets declare it as `data`) visualized as following:

{
 "apple": {
           "it_department": {
			     "head":{
				    "name": "David T. Faker",
				    ...
                                    },
			    },
           "r&d_department":{
 			     ...
			    },
	   ...
          },
 ...
}

and example list(lets declare it as `keys`) of keys and its expected return are as following:

Example1:
 keys = ["apple", "it_department", "head", "name"]
 expectd_return_value = "David T. Faker"

Example2:
 keys = ["apple", "it_department", "head"]
 expectd_return_value = {
			 "name": "David T. Faker",
			 ...
                        }

Example3:
 keys = ["apple"]
 expected_return_value = {
                           "it_department": {
                        		     "head":{
                        			    "name": "David T. Faker",
                        			    ...
                                                    },
                        		    },
                           "r&d_department":{
                        		     ...
                        		    },
                           ...
                          },
```
>This is an abstraction of the real problem we had in production. All the data here are fake and made up by myself. All explanation rights reserved.

Take a few seconds and think about how would you solve this problem?  
  
As to me, it is simple and straightforward:

It is a recursive problem: start from first key in the list, I get the value of it from the JSON, and feed the value and the rest of the list into function itself for further process. Then the base case it when current key is last item in the list, we simply return the value of that key from current JSON.  
  
Before I post my solution, let me share the one line solution from the code I was reviewing:  
  
```python
    def get_value_from_keys(data, keys):
        return reduce(operator.getitem, keys, data)
```

You might as well take a few seconds to see why this work, :)  
  
Without futher ado, let me share my solution first before I clarify why the above solution works and what is missing.  
  
```python
    def get_value_from_keys(data, keys):
        if len(keys) == 0:
            return []
        elif len(keys) == 1:
            if keys[0] in data:
                return data[keys[0]]
            else:
                return []
        return get_value_from_keys(data[keys[0]], keys[1:])
```

As mine is a pretty straightforward recursive solutoin, no need to talk further on it. Let me briefly clarify why the logic of one line soluton is right and also comment on why it is complete or great to put into production code.  
  
To understand the one line solution, you will first need knowledge of python's built-in `reduce()`, `operator.getitem()`.  
  

