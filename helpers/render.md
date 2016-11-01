# Render

[Render](https://github.com/qor/render) support to render templates by your control.

## Usage

```go
import "github.com/qor/render"

func main() {
  Render := render.New()

  Render.Execute("index", obj, request, writer)

  Render.Layout("application").Execute("index", obj, request, writer)

  Render.Layout("application").Funcs(funcsMap).Execute("index", obj, request, writer)
}
```
