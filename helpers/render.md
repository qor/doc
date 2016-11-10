# Render

[Render](https://github.com/qor/render) support to render templates by your control.

## Usage

First, Import [Render](https://github.com/qor/render) and initialize it

```go
import "github.com/qor/render"

func main() {
  Render := render.New()
}
```

Invoke `Execute` function to render your template.

```go
  Render.Execute("index", context, request, writer)
```

The `Execute` function accepts 4 parameters

1. The template name, The default view path is `{current_repo_path}/app/views`, So in this example, [Render](https://github.com/qor/render) will looking for the template `{current_repo_path}/app/views/index.tmpl`. If the parameter is `users/profile`, the template path shall be `{current_repo_path}/app/views/users/profile.tmpl`.
2. The context you can use in the template. It is a `interface{}` and recorded in `.Result`. For instance, if you pass `context["CurrentUserName"] = "ThePlant"` as the context. In the template, you can call `{% raw %}{{.Result.CurrentUserName}}{% endraw %}` to get the value "ThePlant".
3. [http.Request](https://golang.org/pkg/net/http/#Request) of Go
4. [http.ResponseWriter](https://golang.org/pkg/net/http/#ResponseWriter) of Go

### Specify layout explicitly

The default layout is `{current_repo_path}/app/views/layouts/application.tmpl`. If you want use other layout like `new_layout`. You can pass it as a parameter to `Layout` function.

```go
  Render.Layout("new_layout").Execute("index", context, request, writer)
```

[Render](https://github.com/qor/render) will find layout at `{current_repo_path}/app/views/layouts/new_layout.tmpl`.

### Render with helper functions

Sometime you may want to have some helper functions in your template. [Render](https://github.com/qor/render) support to pass helper functions by `Funcs` function.

```go
  Render.Funcs(funcsMap).Execute("index", obj, request, writer)
```

The `funcsMap` is based on [html/template.FuncMap](https://golang.org/src/html/template/template.go?h=FuncMap#L305). So with

```go
  funcMap := template.FuncMap{
    "Greet": func(name string) string { return "Hello " + name },
  }
```

You can call this in the template

```go
{{Greet "ThePlant" }}
```

The output is `Hello ThePlant`.
