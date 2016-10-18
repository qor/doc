# Select one

## Configurations

| Name | Type | Default | Description |
| --- | --- | --- | --- |
| Collection | interface{} |  | Specify the options of the select, Accept 1 or 2 dimensional array |
| AllowBlank | bool | false | Add a close icon beside the selector, user could click it to clear current selection |
| SelectionTemplate | string |  | Accept a file path to overwrite default selector template. Default path is `/metas/form/select_one`. Check examples for usage. |
| SelectMode | string | "select" | Set the data source of the options, Three available options "select", "select_async" and "bottom_sheet". Check examples for detail |
| Select2ResultTemplate | template.JS |  | Same as select2's option [templateResult](https://select2.github.io/options.html#can-i-change-how-the-placeholder-looks) |
| Select2SelectionTemplate | template.JS |  | Same as select2's option [templateSelection](https://select2.github.io/options.html#templateSelection) |
| RemoteDataResource | *Resource |  | For the configuration `SelectMode`, when it set to "select_async" or "bottom_sheet", this will be the data resource, check examples for usage detail |

## Examples

Change the Meta type of `Gender` field in User resource from `string` (default value) to `select_one`, with options `M` | `F`

```go
user.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}}})
```

Add clear icon to clear current selection

```go
u.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}, AllowBlank: true}})
```

Overwrite default SelectionTemplate

```go
u.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}, SelectionTemplate: "metas/form/customised_select_one.tmpl"}})
```

The full path of `SelectionTemplate` in example is `app/views/qor/metas/form/customised_select_one.tmpl`. About how to define/use customise template. Check [QOR view paths](../chapter2/theme.md#customize-views) document.

TODO: SelectMode: `select_async`, `bottom_sheet` with `remoteDataResource`.

```
u.Meta(&admin.Meta{Name: "Gender", Type: "select_one", Config: &admin.SelectOneConfig{Collection: []string{"M", "F"}, SelectMode: `select_async` }})
```
