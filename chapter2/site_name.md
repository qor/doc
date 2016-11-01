# Site Name

Use `SetSiteName` to set [QOR Admin](https://github.com/qor/admin)'s HTML title.

```go
Admin.SetSiteName("QOR DEMO")
```

The name will also be used to auto-load javascripts and stylesheet files that you can provide for customizing the [QOR Admin](https://github.com/qor/admin) interface.

For example, say you set the site name as `QOR Demo`, [QOR Admin](https://github.com/qor/admin) will look up `qor_demo.js`, `qor_demo.css` in [QOR view paths](../chapter2/theme.md#customize-views), and load them if present.
