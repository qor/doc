## Resource

#### Add a resource

```
  user := Admin.AddResource(&User{}, &admin.Config{Menu: []string{"User Management"}})
```

Configurations:

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| Name | string |  | Display name of the resource. |
| Menu | []string |  | Menu setting of the resource, check [Menus](../chapter2/menus.md#h1) for detail. |
| Invisible | bool | false | Set whether the resource is visible in menu. |
| Priority | int |  | Control the display sequence in menu, ASC. |
| PageCount | int |  | How many records shall be shown per page. |
| Singleton | bool | false | Set the resource is a single object or multiple objects. e.g. "SEO setting" vs "Users". |
| Permission | *roles.Permission |  | Control authority of the resource. check [Authority]() for - detail |
| Themes | []ThemeInterface |  | Set customized theme for the resource, check [Theme](../chapter2/- theme.md#h1) for detail. |

#### Set field type

This set how the field of resource being managed in the QOR admin interface.

```
  user.Meta(&admin.Meta{Name: "Gender", Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female", "Unknown"}}})
```

TODO: common configuration of meta, like Label name, sequence

Here's the meta we supported

- [text input]()
- [select one]()
- [select many]()
- [file upload]()
- [rich text editor]()
- ...

#### Set CRUD attributes

This set what attributes of resource shall be shown in the index, new, edit, show page.

```go
// Set attributes will be shown in the index page
// show given attributes
order.IndexAttrs("User", "PaymentAmount", "ShippedAt", "CancelledAt", "State", "ShippingAddress")
// show all attributes except `State`
order.IndexAttrs("-State")

// Set attributes will be shown in the new page
order.NewAttrs("User", "PaymentAmount", "ShippedAt", "CancelledAt", "State", "ShippingAddress")
// show all attributes except `State`
order.NewAttrs("-State")
// Structure the new form to make it tidy and clean with `Section`
product.NewAttrs(
  &admin.Section{
    Title: "Basic Information",
    Rows: [][]string{
      {"Name"},
      {"Code", "Price"},
    }
  },
  &admin.Section{
    Title: "Organization",
    Rows: [][]string{
      {"Category", "Collections", "MadeCountry"},
    }
  },
  "Description",
  "ColorVariations",
}

// Set attributes will be shown for the edit page, similar to new page
order.EditAttrs("User", "PaymentAmount", "ShippedAt", "CancelledAt", "State", "ShippingAddress")

// Set attributes will be shown for the show page, similar to new page
// If ShowAttrs haven't been configured, there will be no show page generated, by will show the edit from instead
order.ShowAttrs("User", "PaymentAmount", "ShippedAt", "CancelledAt", "State", "ShippingAddress")
```

#### General settings

- [Search](../chapter2/search.md#h1)
- [Scopes and Filters](../chapter2/filter.md#h1)
- [Actions](../chapter2/actions.md#h1)
- [Theme & Customize views](../chapter2/theme.md#h1)


