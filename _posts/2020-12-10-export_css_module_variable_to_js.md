---
layout: post
label: til
title: "Export css module variable to JS"
---

<p>
  
  	<span class="issue-label" style="background-color: c5def5">ReactJS</span>
  
  	<span class="issue-label" style="background-color: 792da8">css</span>
  
  	<span class="issue-label" style="background-color: f298a4">js</span>
  
</p>
```scss
// styles.module.scss
:export {
  grayLightColor: $color-gray-light;
}
```

```js
import styles from './styles.module.scss';

console.log(styles.grayLightColor);
```

