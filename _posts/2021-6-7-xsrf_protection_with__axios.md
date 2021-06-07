---
layout: post
label: til
title: "XSRF protection with Axios"
---

<p>
  
</p>
```javascript
const options = {
  method: 'post',
  url: '/signup',
  xsrfCookieName: 'XSRF-TOKEN',
  xsrfHeaderName: 'X-XSRF-TOKEN',
};

// send the request
axios(options);
```

