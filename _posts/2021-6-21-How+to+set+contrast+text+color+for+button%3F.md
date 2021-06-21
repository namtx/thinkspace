---
layout: post
label: til
title: "How to set contrast text color for button?"
---

<p>
  
  <span class="issue-label" style="background-color: #792da8">css</span>
  
</p>
```scss
@function color-yiq(
  $color,
  $support-black: $neutral-gray-dark,
  $white: $yiq-text-light
) {
  $r: red($color);
  $g: green($color);
  $b: blue($color);

  $yiq: (($r * 299) + ($g * 587) + ($b * 114)) / 1000;

  @if ($yiq >= $yiq-contrasted-threshold) {
    @return $support-black;
  } @else {
    @return $white;
  }
}
```

