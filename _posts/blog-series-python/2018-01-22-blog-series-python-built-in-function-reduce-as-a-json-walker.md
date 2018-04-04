---
layout: "post"
title: "Python's Built-in function Reduce() as a JSON walker"
comments: true
categories: python
tags: [blogseries, python, dummynotes]
image: "blog-series-python/blog-series-davids-learning-notes-as-a-python-dummy.png"
---


I was doing a code review, and an use of python's built-in `Reduce()` came to my attention. For the same problem, I would definitely go with a simple recursive soluton, which is straightforward and easy to maintain(a.k.a understand). But the author of the code used python's built-in function `Reduce()`. I didn't get it at first, but managed to grasp it with some hassile of googling.  
  
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

It is a recursive problem: start from first key in the list, I get the value of it from the JSON, and feed the value and the rest of the list into function itself for further process. Then the base case is: when current key is last item in the list, we simply return the value of that key from current JSON.  
  
Before I post my solution, let me share the one line solution from the code I was reviewing:  
  
```python
    def get_value_from_keys(data, keys):
        return reduce(operator.getitem, keys, data)
```

You might as well take a few seconds to see why this work, :)  
  
Without futher ado, let me share my solution first before I clarify why the above solution works and share opinions on it.  
  
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

As mine is a pretty straightforward recursive solutoin, no need to talk further on it. Let me briefly clarify why the logic of one line soluton is right.  
  
To understand the one line solution, you will first need knowledge of python's built-in `reduce()`, `operator.getitem()`. check out official document for exhausted explanation on these two functinos.  
  
As in my words:  
  
`reduce(func, list)` usually take a function of two parameters, and a list. It will consume first two items and produce the return value into the list. Then list size reduce by one with a new 'aggregated' value to replace the original first two items( The function, as the first parameter in `reduce()` defines how to aggregate). Following such execution fashion, `reduce()` repeat the same operation on the remaining list until there is only item left in the list and return it as the final result of `reduce()`.  
  
`operator.getitem` (complete signature: `getitem(A, B)`) is a built-in function, which comes in handy here as what we exactly wanted:get the value of B in A. In our case, it is in the form of `getitem(json, key)` for each operation of `reduce`.  
  
One more thing need to be pointed out here as to explaining the one line solution more thoroughly:  
  
`reduce(func, list)` can also take a third parameter, which is named as `initializer` in official documentation. It looks like:  
```python
reduce(func, list[, initializer])
```
The initializer will be prepended into list when it is empty, which is sort of self-explanatory as by its name.  
  
Now, is it more clear why the previous one line solution works ?  
  
So it takes a single JSON as initializer, which then is pre-appended into the list of keys. So, every time, the funciton get a value of first key(second item in the list) in the JSON(first item in the list). Finally, functions stops when there is only one item left in the JSON. It is first item and the only item, and it is exactly what we wanted: the value from the keys path in the nested json.  
  
I made an illustration through sketch to show how you can visualize the list when it is being processed by `reduce()`:  
  
![visual steps on how the list is being processed in reduce function](/assets/img/blog-series-python/series-steps-json-walker-demonstration.png)
  
In the end, I prefer my naive recursive solution. A few points worth pointing out for the online solution:  
  
Pros:  
  
* Neat, and pretty.
* Pushing reviewers to learn sth new is he/she is not familiar with reduce&operator.getitem. Thumbs up.


Cons:

* It is not straightforward for people to grasp what it is at first hand when they are not familiar with functions as such.
* In fact, I think that using the json data as `initializer` is a misuse. Simply pre-appending the json into the list would greatly reduce the understanding gap here.  
  
Feel free to comment below. - Lets discuss !
