# ActionBar

[ActionBar](https://github.com/qor/action_bar) depends on [QOR Admin](../chapter2/setup.md). It provides an action bar on the top of frontend page. The bar contains:

* Switcher of `Preview` and `Edit` mode
* Login/Logout links
* Additional links in a menu

[![GoDoc](https://godoc.org/github.com/qor/action_bar?status.svg)](https://godoc.org/github.com/qor/action_bar)

## Usage

```go
import "github.com/qor/admin"
import "github.com/qor/action_bar"

func main() {
  Admin := admin.New(&qor.Config{DB: db})

  // Register Global ActionBar object
  // Auth is admin.Auth interface, you need to define a struct and implements interface's functions
  ActionBar = action_bar.New(Admin, Auth)
  ActionBar.RegisterAction(&action_bar.Action{Name: "Admin Dashboard", Link: "/admin"})

  // Then use Render to render action bar in view
  ActionBar.Render(writer, request)

  // Make resource able be edit in frontend directly
  // 1. Add action_bar funcmap to view
  //    ActionBar.FuncMap(writer, request)
  // 2. Using funcmap render_edit_button in template
  //    {{ render_edit_button .Product }}
  // 3. then you will see a `Edit` button in product show page and user could edit product' info in frontend
}

```

[Online Demo](http://demo.getqor.com/), you will see a bar at the top of homepage.
