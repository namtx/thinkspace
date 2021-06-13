---
layout: post
label: til
title: "How to pass a function into useState in ReactJS"
---

<p>
  
  <span class="issue-label" style="background-color: #c5def5">ReactJS</span>
  
</p>
### Problem
As we already know, the Dispatch function from the `useState` hook accept a function as an async dispatcher. So, we can't pass a function into this dispatcher as expected. Example:

```typescript
const [callback, setCallback] = useState<() => void>();
// then
setCallback(() => doSomething()); // ❌  we can't do this, as the returned value of `doSomething` will be pass into the `callback` value.
```

### Solution
We need to pass a function that returns a function that returns a function 😂 

```typescript
const [callback, setCallback] = useState<() => void>();
// then
setCallback(() => () => doSomething());  // ✅ correct!
```



