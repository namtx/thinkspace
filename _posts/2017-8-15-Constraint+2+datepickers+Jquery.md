---
layout: post
label: til
title: "Constraint 2 datepickers Jquery"
---

<p>
  
</p>
```js
$('document').ready(function() {
  $('#service_start_date, #service_end_date').datepicker({
    beforeShow: customRange
  });
});

function customRange(input) {
  if (input.id === 'service_end_date') {
    return {
      minDate: $('#service_start_date').datepicker("getDate")
    };
  } else if (input.id === 'service_start_date') {
    return {
      maxDate: $('#service_end_date').datepicker("getDate")
    };
  }
}
```

