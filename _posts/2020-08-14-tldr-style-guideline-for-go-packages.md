---
layout: post
title: TL;DR - Style guideline for Go packages
description: TL;DR - Style guideline for Go packages
keywords: 
---

- Use multiple files in same packages

```
- doc.go       // package documentation
- headers.go   // HTTP headers types and code
- cookies.go   // HTTP cookies types and code
- http.go      // HTTP client implementation, request and response types, etc.
```

- Organize by responsibility

Bad:

```go
package models // DON'T DO IT!!!

// User represents a user in the system.
type User struct {...}
```

Good: 

```go
package mngtservice

// User represents a user in the system.
type User struct {...}

func UsersByQuery(ctx context.Context, q *Query) ([]*User, *Iterator, error)

func UserIDByEmail(ctx context.Context, email string) (int64, error)
```

### Package naming
- Lowercase only
- Short, but representative names
- Avoid overly broad package name like "common" and "util"

Bad:
```go
import "pkgs.org/common" // DON'T!!!
```

- Clean import path
- No plurals
- Renames should follow the same rules. If you are renaming the standard library, it is nice to add a `go` prefix:

```go
import (
    gourl "net/url"

    "myother.com/url"
)
```

- Enforce vanity URLs 
- Package documentation
- `go doc`

### References:
- [https://rakyll.org/style-packages/](https://rakyll.org/style-packages/)
