---
categories:
  - programming
  - python
tags: python
layout: post
title: Using Python to Output Pretty Looking JSON
created: 1482036547
---

Every once and awhile, theirs occasions that I have a giant glob of JSON data that I want to easily read it's data. Normally I opted to use <a href="http://jsonlint.com/" taget="_blank">http://jsonlint.com/</a>. The problem is whenever I use  <a href="http://jsonlint.com/" taget="_blank">http://jsonlint.com/</a>, I always have to be sure the JSON data doesn't include anything confidential. I was happy to learn that you can easily use the <a href="https://docs.python.org/3/library/json.html" target="_blank">json library</a> in Python to accomplish essentially the same thing.

For example:

```bash
alpha03:~ tony$ cat test.json
{"employees":[{"firstName":"John", "lastName":"Doe"},{"firstName":"Anna", "lastName":"Smith"},{"firstName":"Peter", "lastName":"Jones"}]} 
```

```bash
alpha03:~ tony$ python -m json.tool < test.json
{
    "employees": [
        {
            "firstName": "John",
            "lastName": "Doe"
        },
        {
            "firstName": "Anna",
            "lastName": "Smith"
        },
        {
            "firstName": "Peter",
            "lastName": "Jones"
        }
    ]
}
```
