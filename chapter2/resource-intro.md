## Resource

You can think of a Resource as a representation of any similar set of data that is used in your system, mostly that is content that will manifest on the front-end.

### Add a resource to [QOR Admin](https://github.com/qor/admin)

```
  type User struct {
    gorm.Model
    Email     string
    Password  string
    Name      sql.NullString
    Gender    string
    Role      string
    Addresses []Address
  }

  user := Admin.AddResource(&User{}, &admin.Config{Menu: []string{"User Management"}})
```

#### Configurations

Available options inside `admin.Config` are:

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| Name | string |  | Display name of the resource. |
| Menu | []string |  | Menu setting of the resource, check [Menus](../chapter2/menus.md#h1) for detail. |
| Invisible | bool | false | Set whether the resource is visible in menu. |
| Priority | int |  | Control the display sequence in menu, ordered by ASC. |
| PageCount | int |  | Pagination setting, Set how many records shall be shown per page. |
| Singleton | bool | false | Set the resource is a single object or multiple objects. e.g. "SEO setting" vs "Users". |
| Permission | *roles.Permission |  | Control the authority of the resource. check [Roles](../plugins/roles.md) for detail |
| Themes | []ThemeInterface |  | Set customized theme for the resource, check [Theme](../chapter2/theme.md#h1) for detail. |

#### Set CRUD attributes

This set what attributes of resource shall be shown in the index, new, edit, show page.

```go
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

### Field type

By default, Resource fields in [QOR Admin](https://github.com/qor/admin) are rendered based on its types and relations. The default should satisfy the usual cases, you can customize the rendering by overwritting the `Meta` definition.

There are some `Meta` types that have been predefined, including `string`, `password`, `date`, `datetime`, `rich_editor`, `select_one`, `select_many` and so on (see full list below). [QOR Admin](https://github.com/qor/admin) will auto select a type for `Meta` based on a field's data type. For example, if a field's type is `time.Time`, [QOR Admin](https://github.com/qor/admin) will determine `datetime` as the type.

Here's an example about make "Gender" of user as a select element in the user form that include three options "Male", "Female" and "Unknown".

```
  user.Meta(&admin.Meta{Name: "Gender", Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female", "Unknown"}}})
```

#### Configurations {#meta-config}

| Name | Type | Description |
| --- | --- | --- |
| Name | string | The name of the attribute |
| Type | string | The display type of the attribute, e.g. select_one, password.|
| Label | string | The label of the attribute in the form. The default label of "address" is "Address". You can set another label for "address" by this option.|
| FieldName | string | Mapping to the attribute name in the resource, Usually, This is no need to set and same with Name by default. |
| Setter| func(resource interface{}, metaValue *resource.MetaValue, context *qor.Context) | Overwrite default "Setter" of [QOR Admin](https://github.com/qor/admin) to customize the value before save it to database. |
| Valuer | func(interface{}, *qor.Context) interface{} | Overwrite default "Getter" logic to customize the value fetched from the database. |
| FormattedValuer | func(interface{}, *qor.Context) interface{} | Customize the output of the attribute in index page and API. |
| Resource | *Resource | Used for customize attribute in nested form, Check [How to customize attribute in nested form](../metas/collection-edit.md) for detail. |
| Permission | *roles.Permission | Define user authority of this attribute, Check [Roles](../plugins/roles.md) for detail. |
| Config | MetaConfigInterface | The configuration of current type of attribute, e.g. `Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female", "Unknown"}}`. Check each meta document for detail. |

**Predefined meta types**:

- [Checkbox](../metas/checkbox.md)
- [Collection/Single edit](../metas/collection-edit.md)
- [Date/Datetime](../metas/date.md)
- [Number/Float](../metas/number.md)
- [Hidden/Readonly](../metas/hidden-readonly.md)
- [Password](../metas/password.md)
- [Rich editor](../metas/rich-editor.md)
- [Select many](../metas/select-many.md)
- [Select one](../metas/select-one.md)
- [String/Text](../metas/text-input.md)

#### Nested resource

According to the relationship between resources. [QOR](https://github.com/qor/qor) will generate nested form automatically. Check [Single/Collection edit](../metas/collection-edit.md) for detail.
