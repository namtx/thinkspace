---
layout: post
label: til
title: "Shell script - remove first and last quote from a variable"
---

<p>
  
  <span class="issue-label" style="background-color: #b60205">Unix</span>
  
  <span class="issue-label" style="background-color: #E6F510">bash</span>
  
</p>
Using `tr` 
```bash
$ echo "test" | tr -d '"'
```

[source](https://stackoverflow.com/a/26314887/13211226) 

