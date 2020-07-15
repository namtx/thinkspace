---
layout: post
title: Cobra in 5 minutes
description: Cobra in 5 minutes
keywords: golang
---

### Typical cobra based app

```go
// main.go

package main

import (
  "{pathToYourApp}/cmd"
)

func main() {
  cmd.Execute()
}
```
### Flags parse

First we have define variables

```go
package cmd

var (
  title string
  description string
  rootCmd = &cobra.Command{
    Use: "blabla",
    Run: func(cmd *cobra.Command, args []string) {
      // your logic live there, right here!
      // you can directly access above variables
    },
  }
)

// called by main.go
func Execute() {
  rootCmd.Execute()
}
```

And then, parse them

```go
func init() {
  rootCmd.PersistentFlags().StringVarP(&title, "title", "t", "", "Post title")
  rootCmd.PersistentFlags().StringVarP(&description, "description", "d", "", "Post descritption")
  rootCmd.PersistentFlags().StringVarP(&layout, "layout", "l", "post", "Post layout")
  rootCmd.PersistentFlags().StringArrayVarP(&keywords, "keywords", "k", []string{}, "Post keywords")
}
```
