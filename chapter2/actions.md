# Actions

QOR Admin has the notion of four `Action Modes`:

* Bulk actions (will be shown in index page as bulk actions),
* Edit form action (will be shown in edit page),
* Show page action (will be shown in show page),
* Menu item action (will be shown in table's menu).

You can register an Action of any mode using the `Action` method, along with `Modes` values to contol where to show them:

```go
product.Action(&admin.Action{
  Name: "enable",
  Handle: func(actionArgument *admin.ActionArgument) error {
    // `FindSelectedRecords` => return selected record in bulk action mode, return current record in other mode
    for _, record := range actionArgument.FindSelectedRecords() {
      actionArgument.Context.DB.Model(record.(*models.Product)).Update("disabled", false)
    }
    return nil
  },
  Modes: []string{"index", "edit", "show", "menu_item"},
})
```

## Register Actions need user's input

```go
order.Action(&admin.Action{
  Name: "Ship",
  Handle: func(argument *admin.ActionArgument) error {
    trackingNumberArgument := argument.Argument.(*trackingNumberArgument)
    for _, record := range argument.FindSelectedRecords() {
      argument.Context.GetDB().Model(record).UpdateColumn("tracking_number", trackingNumberArgument.TrackingNumber)
    }
    return nil
  },
  Resource: Admin.NewResource(&trackingNumberArgument{}),
  Modes: []string{"show", "menu_item"},
})

// the ship action's argument
type trackingNumberArgument struct {
  TrackingNumber string
}
```

## Use `Visible` to hide registered Action in some case

```go
order.Action(&admin.Action{
  Name: "Cancel",
  Handle: func(argument *admin.ActionArgument) error {
    // cancel the order
  },
  Visible: func(record interface{}) bool {
    if order, ok := record.(*models.Order); ok {
      for _, state := range []string{"draft", "checkout", "paid", "processing"} {
        if order.State == state {
          return true
        }
      }
    }
    return false
  },
  Modes: []string{"show", "menu_item"},
})
```
