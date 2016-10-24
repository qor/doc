# Select Many

## Configurations

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| Collection | interface{} |  | Specify the options of the select, Accept 1 or 2 dimensional array |
| SelectionTemplate | string |  | Accept a file path to overwrite default selector template. Default path is `/metas/form/select_one`. Check examples for usage. |
| SelectMode | string | "select" | Set the data source of the options, Three available options "select", "select_async" and "bottom_sheet". Check examples for detail |
| Select2ResultTemplate | template.JS |  | Same as select2's option [templateResult](https://select2.github.io/options.html#can-i-change-how-the-placeholder-looks) |
| Select2SelectionTemplate | template.JS |  | Same as select2's option [templateSelection](https://select2.github.io/options.html#templateSelection) |
| RemoteDataResource | *Resource |  | For the configuration `SelectMode`, when it set to "select_async" or "bottom_sheet", this will be the data resource, check examples for usage detail |

## Examples

### Set option manually

Assume User has many favorite products. Like

```
type User struct {
  gorm.Model
  Name              string
  Gender            string
  FavouriteProducts []string
}
```

Then add two brands "ASICS" and "Lacoste" as candidates.

```go
user.Meta(&admin.Meta{Name: "FavouriteProducts", Type: "select_many", Config: &admin.SelectManyConfig{Collection: []string{"ASICS", "Lacoste"}}})
```

### Use association data

You can use other resource's data as options. Assume a user has some favorite products, and there is a product "Product a" existing in the database. QOR could do this automatically with zero configuration.

```
type User struct {
  gorm.Model
  Name              string
  Gender            string
  FavouriteProducts []Product
}

type Product struct {
  gorm.Model
  Name        string
  Description string
}

user.Meta(&admin.Meta{Name: "FavouriteProducts", Type: "select_many"})
```

This will looks like:

![select many: nested form](select_many_with_other_objects.png)


### Overwrite default SelectionTemplate

```go
u.Meta(&admin.Meta{Name: "FavouriteProducts", Type: "select_many", Config: &admin.SelectManyConfig{Collection: []string{"ASICS", "Lacoste"}, SelectionTemplate: "metas/form/customised_select_many.tmpl"}})
```

The full path of `SelectionTemplate` in example is `app/views/qor/metas/form/customised_select_many.tmpl`. About how to define/use customise template. Check [QOR view paths](../chapter2/theme.md#customize-views) document.

### Different select mode

TODO: SelectMode: `select_async`, `bottom_sheet` with `remoteDataResource`.

```
u.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}, SelectMode: `select_async` }})
```
