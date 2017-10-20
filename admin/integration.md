# Integrate with WEB frameworks

QOR Admin should be able to integrate with most golang web frameworks (let me know if there are any web framework not supported), here are some examples for how to integrate with them.

## Integrate with HTTP ServeMux

```go
mux := http.NewServeMux()
Admin.MountTo("/admin", mux)
http.ListenAndServe(":9000", mux)
```

## Integrate with Beego

```go
mux := http.NewServeMux()
Admin.MountTo("/admin", mux)

beego.Handler("/admin/*", mux)
beego.Run()
```

## Integrate with Gin

```go
mux := http.NewServeMux()
Admin.MountTo("/admin", mux)

r := gin.Default()
r.Any("/admin/*", gin.WrapH(mux))
r.Run()
```

## Integrate with gorilla/mux

```go
adminMux := http.NewServeMux()
Admin.MountTo("/admin", adminMux)

r := mux.NewRouter()
r.PathPrefix("/admin").Handler(adminMux)

http.Handle("/", r)
```
