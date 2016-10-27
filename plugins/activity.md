# Activity

Activity provides QOR Admin with an activity tracking feature for any Resource.

Applying Activity to a Resource will add `Comment` and `Track` data/state changes within the admin interface.

[![GoDoc](https://godoc.org/github.com/qor/activity?status.svg)](https://godoc.org/github.com/qor/activity)

## Usage

```go
import "github.com/qor/admin"

func main() {
  Admin := admin.New(&qor.Config{DB: db})
  order := Admin.AddResource(&models.Order{})

  // Register Activity for Order resource
  activity.Register(order)
}
```

This will add activity tracking feature to order like:

![activity demo](activity-demo.png)

[Online Demo](http://demo.getqor.com/admin/orders)
