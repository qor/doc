# Select one

## Configurations

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| Collection | interface{} |  | Specify the options of the *select*. Accepts a 1 or 2 dimensional array |
| AllowBlank | bool | false | Add a close icon beside the *select*, which clears the current selection when clicked |
| SelectionTemplate | string |  | Accept a file path to overwrite default *select* template. Default path is `/metas/form/select_one` |
| SelectMode | string | "select" | Set the data source of the options: "select", "select_async", or "bottom_sheet" |
| Select2ResultTemplate | template.JS |  | Same as [select2](https://select2.github.io)'s option [templateResult](https://select2.github.io/options.html#can-i-change-how-the-placeholder-looks) |
| Select2SelectionTemplate | template.JS |  | Same as [select2](https://select2.github.io)'s option [templateSelection](https://select2.github.io/options.html#templateSelection) |
| RemoteDataResource | *Resource |  | Works in conjunction with the`SelectMode` configuration, when set to "select_async" or "bottom_sheet" the value represents the data resource |

## Examples

### Set option manually

In a hypothetical project, to change the Meta type of the `Gender` field in User resource from `string` (default value) to `select_one`, with options `M` | `F`, one might use the following code:

```go
user.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}}})
```

### Add clear icon to clear current selection

```go
u.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}, AllowBlank: true}})
```

### Generate options by data from the database

This is an example of generates options by the languages in the database. The `Collection` option accept a function with `context *admin.Context` parameter. You can get the database by `context.GetDB()` function. The returned value should be a `[][]string` array.

```go
  user.Meta(&admin.Meta{Name: "Language", Label: "Locale", Type: "select_one",
    Config: &admin.SelectOneConfig{
      Collection: func(_ interface{}, context *admin.Context) (options [][]string) {
        var languages []models.Language
        context.GetDB().Find(&languages)

        for _, n := range languages {
          idStr := fmt.Sprintf("%d", n.ID)
          var option = []string{idStr, n.NameWithCountry()}
          options = append(options, option)
        }

        return options
      },
    },
  })
```

### Overwrite default SelectionTemplate

```go
u.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}, SelectionTemplate: "metas/form/customised_select_one.tmpl"}})
```

The above code snippet is taken from the [qor-example project](https://github.com/qor/qor-example). The full path of the `SelectionTemplate` in [qor-example](https://github.com/qor/qor-example) is `app/views/qor/metas/form/customised_select_one.tmpl`. For more about how to define/use custome templates, have a look at [QOR view paths](../chapter2/theme.md#customize-views).
