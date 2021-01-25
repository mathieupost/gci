# GCI

GCI, a tool that control golang package import order and make it always deterministic.

It handles empty lines more smartly than `goimport` does.

## Download

```shell
$ go get github.com/mathieupost/gci
```

## Usage

```shell
$ gci -h
usage: gci [flags] [path ...]
  -d	display diffs instead of rewriting files
  -local string
    	put imports beginning with this string after 3rd-party packages, only support one string
  -w	write result to (source) file instead of stdout
```

## Examples

Run `gci -w -local github.com/mathieupost/gci main.go` and you will handle following cases.

### simple case

```go
package main
import (
  "golang.org/x/tools"

  "fmt"

  "github.com/mathieupost/gci"
)
```

to

```go
package main
import (
  "fmt"

  "golang.org/x/tools"

  "github.com/mathieupost/gci"
)
```

### with alias

```go
package main
import (
  "fmt"
  go "github.com/golang"
  "github.com/mathieupost"
)
```

to

```go
package main
import (
  "fmt"

  go "github.com/golang"

  "github.com/mathieupost/gci"
)
```

### with comment and alias

```go
package main
import (
  "fmt"
  _ "github.com/golang" // golang
  "github.com/mathieupost"
)
```

to

```go
package main
import (
  "fmt"

  // golang
  _ "github.com/golang"

  "github.com/mathieupost/gci"
)
```

### with above comment and alias

```go
package main
import (
  "fmt"
  // golang
  _ "github.com/golang"
  "github.com/mathieupost"
)
```

to

```go
package main
import (
  "fmt"

  // golang
  _ "github.com/golang"

  "github.com/mathieupost/gci"
)
```

## TODO

- Support multi-3rd-party packages
- Support multiple lines of comment in import block
- Add testcases
