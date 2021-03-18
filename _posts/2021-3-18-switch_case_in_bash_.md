---
layout: post
label: til
title: "Switch case in bash "
---

<p>
  
</p>
```bash
conf() {
  case $1 in
    nvim)
      nvim ~/.config/nvim/init.vim
      ;;
    skhd)
      nvim ~/.config/skhd/skhdrc
      ;;
    kitty)
      nvim ~/.config/kitty/kitty.conf
      ;;
    zsh)
      nvim ~/.zshrc
      ;;
    *)
      nvim $1
      ;;
  esac
}
```


