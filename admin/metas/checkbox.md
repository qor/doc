# Checkbox

Allows a `bool` attribute to be rendered as a checkbox, for example:

```
type User struct {
  ReceivePromotionEmail bool
}
```

You can set other parameters, such as "label":

```
user.Meta(&admin.Meta{Name: "ReceivePromotionEmail", Label: "Receive email"})
```

Check [available configurations](../chapter2/resource-intro.md#meta-config) for more detail.
