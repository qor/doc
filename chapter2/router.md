# Router

You can define your own routes by `Router`.

## Registering HTTP routes

First, Get `router` from [QOR Admin](https://github.com/qor/admin).

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
