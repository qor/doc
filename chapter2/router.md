# Router

## Registering HTTP routes

Qor admin uses Qor's Router.

```go
router := Admin.GetRouter()

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

// naming route
router.Get("/path/:name", func(context *admin.Context) {
    context.Request.URL.Query().Get(":name")
})

// regexp support
router.Get("/path/:name[world]", func(context *admin.Context) { // "/hello/world"
    context.Request.URL.Query().Get(":name")
})

router.Get("/path/:name[\\d+]", func(context *admin.Context) { // "/hello/123"
    context.Request.URL.Query().Get(":name")
})
```
