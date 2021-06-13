---
layout: post
label: til
title: "vim-surround cheatsheets"
---

<p>
  
</p>
- `ds` - delete surrounding

🔔 the `*` is used to denote the current cursor place

 | Old text | Command | New text ~ |
|---- | ----- | ------ | 
| "Hello *world!" | ds" | Hello world!|
| (123+4*56)/2  | ds)| 123+456/2 |
| <div>Yo!*</div> | dst | Yo! |
- `cs` - change surrounding
- `ys` - "you surround"

- `S` - visual mode

`Shift + V` to go to Visual mode, and `S<div class="important">` to wrap content inside. 🚀 

💡 `d`elete everything `i`nside `"` by sequence: `di"` or anything surround

