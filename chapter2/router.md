# Router

You can define your own routes using `Router`.

Routes (a.k.a. mux, handlers) are a way to map from a URL path to some code which is executed when an end-user accesses that path.

## Registering HTTP routes

First, get `router` from [QOR Admin](../chapter2/setup.md)...

```go
router := Admin.GetRouter()
```

### General routes

```go
router.Get("/path", func(context *admin.Context) {
    // do something here
})

router.Post("/path", func(context *admin.Context) {
    // do something here
})

router.Put("/path", func(context *admin.Context) {
    // do something here
})

router.Delete("/path", func(context *admin.Context) {
    // do something here
})
```

### Naming route

```go
router.Get("/path/:name", func(context *admin.Context) {
    context.Request.URL.Query().Get(":name")
})
```

### Regexp support

```go
router.Get("/path/:name[world]", func(context *admin.Context) { // "/hello/world"
    context.Request.URL.Query().Get(":name")
})

router.Get("/path/:name[\\d+]", func(context *admin.Context) { // "/hello/123"
    context.Request.URL.Query().Get(":name")
})
```
