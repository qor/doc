# Dashboard

QOR Admin provides a default dashboard page with some dummy text. If you want to customize the dashboard, you can create a file `dashboard.tmpl` in [QOR view paths](#customizing-views), QOR Admin will load it as golang templates when rendering the dashboard.

If you want to disable the dashboard, you can redirect it to some other page, for example:

```go
Admin.GetRouter().Get("/", func(c *admin.Context) {
  http.Redirect(c.Writer, c.Request, "/admin/clients", http.StatusSeeOther)
})
```
