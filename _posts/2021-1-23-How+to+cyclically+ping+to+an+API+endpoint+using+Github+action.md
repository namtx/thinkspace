---
layout: post
label: til
title: "How to cyclically ping to an API endpoint using Github action"
---

<p>
  
  <span class="issue-label" style="background-color: #DCEAD6">devops</span>
  
  <span class="issue-label" style="background-color: #22ADD3">hack</span>
  
</p>
- Create a cron-job github action with the following content:
```yaml
name: Cron jobs
on:
  schedule:
  - cron: '0 */1 * * *' # every hour
  workflow_dispatch: # can handly trigger on Actions page
jobs:
  lobsterss:
    runs-on: ubuntu-latest
    steps:
    - name: curl
      uses: wei/curl@master
      with:
        args: https://lobsters-bot.vercel.app/api/index\?channels\=go\&channels\=ruby\&channels\=web\&channels\=security\&channels\=programming\&channels\=networking\&channels\=linux
```


