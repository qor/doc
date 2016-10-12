## Resource

### Add a resource

```
  user := Admin.AddResource(&User{}, &admin.Config{Menu: []string{"User Management"}})
```

#### Configurations

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| Name | string |  | Display name of the resource. |
| Menu | []string |  | Menu setting of the resource, check [Menus](../chapter2/menus.md#h1) for detail. |
| Invisible | bool | false | Set whether the resource is visible in menu. |
| Priority | int |  | Control the display sequence in menu, ASC. |
| PageCount | int |  | How many records shall be shown per page. |
| Singleton | bool | false | Set the resource is a single object or multiple objects. e.g. "SEO setting" vs "Users". |
| Permission | *roles.Permission |  | Control authority of the resource. check [Authority]() for detail |
| Themes | []ThemeInterface |  | Set customized theme for the resource, check [Theme](../chapter2/- theme.md#h1) for detail. |

### Field type

By default, management pages in QOR Admin are rendered based on your resource's fields' data types and relations. The default should satisfy most use cases, however can customize the rendering by overwritting the `Meta` definition.

There are some Meta types that have been predefined, including `string`, `password`, `date`, `datetime`, `rich_editor`, `select_one`, `select_many` and so on (see full list below). QOR Admin will auto select a type for `Meta` based on a field's data type. For example, if a field's type is `time.Time`, QOR Admin will determine `datetime` as the type.

Here's an example about make "Gender" of user as a select element in the user form that include three options "Male", "Female" and "Unknown".

```
  user.Meta(&admin.Meta{Name: "Gender", Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female", "Unknown"}}})
```

#### Configurations

| Name | Type | Description |
| --- | --- | --- |
| Name | string | The name of the attribute of the resource |
| Type | string | The type of the attribute, e.g. select_one, password.|
| Label | string | The label of the attribute in the form. e.g. "address"'s default label is "Address".|
| FieldName | string | Mapping to the attribute name in the struct, usually, it is same with Name. |
| Setter| func(resource interface{}, metaValue *resource.MetaValue, context *qor.Context) | Define what value shall be set by the passed parameter |
| Valuer | func(interface{}, *qor.Context) interface{} | Define how to fetch value from database |
| FormattedValuer | func(interface{}, *qor.Context) interface{} | Pretty format the value, it will be displayed in index and API. |
| Resource | *Resource | Used for define attribute of nested object, Check [How to customize attribute in nested form]() for detail. |
| Permission | *roles.Permission | Define user authority of this attribute |
| Config | MetaConfigInterface | The configuration of current type of attribute, e.g. `Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female", "Unknown"}}`. Check [predefined meta types]() document for detail |

TODO: Introduce nested object

TODO: How to customize attribute in nested form

```
profile := user.Meta(&admin.Meta{Name: "Profile"}).Resource
profile.Meta(&admin.Meta{Name: "Attribute of profile"})
```

Predefined meta types: [qor admin form templates](https://github.com/qor/admin/tree/master/views/metas/form "qor admin form templates"))

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


