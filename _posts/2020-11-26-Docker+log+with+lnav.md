---
layout: post
label: til
title: "Docker log with lnav"
---

<p>
  
  <span class="issue-label" style="background-color: #6ce27c">docker</span>
  
  <span class="issue-label" style="background-color: #0a7f52">logging</span>
  
</p>
### Redirect Docker Logs to File
```bash
docker log -f [container_id] > docker.log
```

### lnav log file
```bash
lnav docker.log
```

