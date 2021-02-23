---
layout: post
label: til
title: "Describe table in PostgresQL "
---

<p>
  
  	<span class="issue-label" style="background-color: f9d0c4">database</span>
  
  	<span class="issue-label" style="background-color: cc3eef">postgresql</span>
  
</p>
```sql
SELECT 
   table_name, 
   column_name, 
   data_type,
   is_nullable
FROM 
   information_schema.columns
WHERE 
   table_name = 'compensations';
```

