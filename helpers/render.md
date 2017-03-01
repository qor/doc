# Render

[Render](https://github.com/qor/render) provides handy controls when rendering templates.

## Usage

First, Import [Render](https://github.com/qor/render) and initialize it...

```go
import "github.com/qor/render"

func main() {
  Render := render.New()
}
```

Next, invoke `Execute` function to render your template...

```go
  Render.Execute("index", context, request, writer)
```

The `Execute` function accepts 4 parameters:

1. The template name. The default view path is `{current_repo_path}/app/views`. In this example [Render](https://github.com/qor/render) will look for the template `{current_repo_path}/app/views/index.tmpl`, so if the parameter is `users/profile`, the template path shall be `{current_repo_path}/app/views/users/profile.tmpl`.
2. The context you can use in the template. It is an `interface{}` and recorded in `.Result`. For example, if you pass `context["CurrentUserName"] = "Memememe"` as the context. In the template, you can call `{% raw %}{{.Result.CurrentUserName}}{% endraw %}` to get the value "Memememe".
3. [http.Request](https://golang.org/pkg/net/http/#Request) of Go.
4. [http.ResponseWriter](https://golang.org/pkg/net/http/#ResponseWriter) of Go.

### Specify layout explicitly

The default layout is `{current_repo_path}/app/views/layouts/application.tmpl`. If you want use another layout like `new_layout`, you can pass it as a parameter to `Layout` function.

```go
  Render.Layout("new_layout").Execute("index", context, request, writer)
```

[Render](https://github.com/qor/render) will find the layout at `{current_repo_path}/app/views/layouts/new_layout.tmpl`.

### Render with helper functions

Sometimes you may want to have some helper functions in your template. [Render](https://github.com/qor/render) supports passing helper functions by `Funcs` function.

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
{{Greet "Memememe" }}
```

The output is `Hello Memememe`.

### Use with [Responder](./responder.md)

Put the [Render](https://github.com/qor/render) inside [Responder](./responder.md) handle function like this.

```go
  func handler(writer http.ResponseWriter, request *http.Request) {
    responder.With("html", func() {
      Render.Execute("demo/index", viewContext, *http.Request, http.ResponseWriter)
    }).With([]string{"json", "xml"}, func() {
      writer.Write([]byte("this is a json or xml request"))
    }).Respond(request)
  })
```
