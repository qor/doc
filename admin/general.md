## General Configuration

There are some configurations could be configured when initialize Admin

```go
type AdminConfig struct {
  SiteName       string
  DB             *gorm.DB
  Auth           Auth
  AssetFS        assetfs.Interface
  SessionManager session.ManagerInterface
  I18n           I18n
  *Transformer
}
```

* SiteName

By Default, Site Name is `Qor Admin`, you can use AdminConfig's `SiteName` to set a new title.

```go
Admin := admin.New(&admin.AdminConfig{SiteName: "Qor Example"})
```

* Auth & SessionManager

[Authentication](../admin/authentication.md)

* I18n

[Internationalization](../admin/i18n.md)

* AssetFS

[Deploy to production](../admin/deploy.md)

## Dashboard

QOR Admin provides a default dashboard page with some dummy text. If you want to customize the dashboard, you can create a file `dashboard.tmpl` in [QOR view paths](#view-paths), QOR Admin will load it as [Golang](http://golang.org/) templates when rendering the dashboard page.

If you want to disable the dashboard, you can redirect it to some other page, for example:

```go
Admin.GetRouter().Get("/", func(c *admin.Context) {
  http.Redirect(c.Writer, c.Request, "/admin/clients", http.StatusSeeOther)
})
```

## Menus

## View Paths
