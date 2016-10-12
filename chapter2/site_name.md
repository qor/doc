# Site Name

Use `SetSiteName` to set QOR Admin's HTML title, the name will also be used to auto-load javascripts and stylesheet files that you can provide for customizing the admin interface.

For example, say you set the Site Name as `QOR Demo`, admin will look up `qor_demo.js`, `qor_demo.css` in [QOR view paths](../chapter2/theme.md#customize-views), and load them if present.

```go
Admin.SetSiteName("QOR DEMO")
```
