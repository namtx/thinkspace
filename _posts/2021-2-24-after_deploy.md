---
layout: post
label: til
title: "After deploy"
---

<p>
  
  <span class="issue-label" style="background-color: #DCEAD6">devops</span>
  
</p>
có mấy thứ thôi:
làm sao để biết dc health của system:
- **uptime** để biết là work or not ---> uptime check, healthcheck solution
- **metrics** để biết tình trạng capacity, state, performance
làm sao để biết được feature này khi running có bung lỗi hay là không?
- **error tracking**: để catch dc exception immediately with more details
- **log system** để store những level khác ERROR như là info, warn, debug nhằm xem dc hết context xung quanh cái error đó
làm sao để biết là feature này nhanh chậm là do đâu? bottle-neck chỗ đít nào của system gây ra perf issue?
- **APM/tracing system**: dùng instrument để identify transaction, móc nối với các linked service như DB/cache để biết dc detail latency khi đi qua từng cụm

làm sao để biết dc tất cả issue bên trên kia kịp thời & đúng lúc ---> oncall/alerting solution, bắn noti, gọi dt đến mình, notification ngay trên smartphone

