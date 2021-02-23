---
layout: post
label: til
title: "Multiple export with dot "
---

<p>
  
  <span class="issue-label" style="background-color: #f298a4">js</span>
  
</p>
### Do you want to use `Sidebar.Item` instead of `SidebarItem`?

```javascript
import Siderbar from './sidebar';
import Item from './sidebar-item';

export default Object.assign(Sidebar, { Item });
```

