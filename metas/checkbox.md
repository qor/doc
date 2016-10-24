# Checkbox

`bool` attribute will be rendered as checkbox directly. like

```
type User struct {
  ReceivePromotionEmail bool
}
```

You can set other parameters like "label" by

```
user.Meta(&admin.Meta{Name: "ReceivePromotionEmail", Label: "Receive email"})
```

Check [available configurations](../chapter2/resource-intro.md#meta-config) for more detail.
