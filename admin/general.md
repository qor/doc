## General Configuration

You could customize Admin with `AdminConfig` struct when initialize it, here are some general configurations:

```go
type AdminConfig struct {
  SiteName       string
  DB             *gorm.DB
  Auth           Auth
  SessionManager session.ManagerInterface
  I18n           I18n
  AssetFS        assetfs.Interface
  *Transformer
}
```

* <a id="sitename"></a>SiteName

  By Default, Site Name is `Qor Admin`, you can use `SiteName` to change site's title.

  ```go
  Admin := admin.New(&admin.AdminConfig{SiteName: "Qor Example"})
  ```

  hint: [You can inject stylesheets, javascripts into your admin site with your site's title](/admin/theming_and_customization.md#global-stylesheet--javascript)

* DB

  `DB` is a GORM DB connection, it is must required, QOR Admin will use the DB to manage data.

* Auth

  Auth is used for [Authentication](/admin/authentication.md)

* SessionManager

  Admin use SessionManager to handle cookies, flash messages, learn to [customize it](/admin/session_manager.md)

* I18n

  [Internationalization](/admin/i18n.md) solution for Admin

* <a id="assetfs"></a>AssetFS

  AssetFS defined how to look up templates when rendering pages, refer [View Paths]((/admin/theming_and_customization.md#view-paths) for more detail, when deploy your site to production, you usually want your application to be standalone executable, check out [Deploy to production](/admin/deploy.md) for how to.

* Transformer

  [Transformer for RESTFul API](/admin/restful_api.md#transformer)

## Dashboard

QOR Admin provides a default dashboard page with some dummy text. If you want to customize the dashboard, you can create a file `dashboard.tmpl` in [QOR view paths](/admin/theming_and_customization.md#view-paths), QOR Admin will load it as Golang templates when rendering the dashboard page.

If you want to disable the dashboard, you can redirect it to some other page, for example:

```go
Admin.GetRouter().Get("/", func(c *admin.Context) {
  http.Redirect(c.Writer, c.Request, "/admin/clients", http.StatusSeeOther)
})
```
