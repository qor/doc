# Extend QOR Admin

QOR Admin aims to to be a flexible, easily extendable, and highly configurable admin framework that could fit most business requirements. In this chapter, we will learn how to extend the admin framework.

## Resource

### Extend QOR Resource

When added a struct to QOR Admin, QOR Admin will check if this struct and its embedded structs implemented interface [ConfigureResourceBeforeInitializeInterface](https://godoc.org/github.com/qor/qor/resource#ConfigureResourceBeforeInitializeInterface) or [ConfigureResourceInterface](https://godoc.org/github.com/qor/qor/resource#ConfigureResourceInterface)

The [`ConfigureResourceBeforeInitializeInterface`](https://godoc.org/github.com/qor/qor/resource#ConfigureResourceBeforeInitializeInterface) interface will be invoked before initialize the resource.

The [`ConfigureResourceInterface`](https://godoc.org/github.com/qor/qor/resource#ConfigureResourceInterface) interface will be invoked after initialize the resource.

So when `AddResource`, the workflow looks like:

```go
type User struct {
}

func (User) ConfigureQorResourceBeforeInitialize(resource.Resourcer) {
  // do some thing before initialize
}

func (User) ConfigureQorResource(resource.Resourcer) {
  // do some thing after initialize
}

user := Admin.AddResource(&User{})
// 1, run User.ConfigureQorResourceBeforeInitialize(user)
// 2, Apply default settings to Resource
// 3, run User.ConfigureQorResource(user)
```

This is helpful when writing QOR Plugins, most plugins are written based on that, for example: [QOR L10n](https://github.com/qor/l10n), [QOR Publish2](https://github.com/qor/publish2)

### Overwrite CURD Handler

QOR Admin generates default CURD Handlers based on GORM's API, if your resource is not a GORM-backend model, you can consider to write your own CRUD handler, like save it into redis or a cache server, like:

```go
res.FindOneHandler = func(result interface{}, metaValues *resource.MetaValues, context *qor.Context) error {
  // find record and decode it to result
}

res.FindManyHandler = func(results interface{}, context *qor.Context) error {
  // find records and decode them to results
}

res.SaveHandler = func(result interface{}, context *qor.Context) error {
  // save result
}

res.DeleteHandler = func(result interface{}, context *qor.Context) error {
  // delete result
}
```

Checkout https://github.com/qor/qor/blob/master/resource/crud.go to get some hints from default implementions

Generate [nested RESTFul API](/admin/restful_api.md#nested-api) is using this feature.

### Attributes

[As you know](/admin/fields.md#customize-visible-fields), you could set index/show/edit/new page's attributes with `IndexAttrs`, `NewAttrs`, `EditAttrs`, `ShowAttrs`.

When you writing plugins, you might has requirements that always show or hide some attributes, `OverrideIndexAttrs`, `OverrideNewAttrs`, `OverrideEditAttrs`, `OverrideShowAttrs` are for the job, you could write it like:

```go
// Each time you configured EditAttrs for the resource, we will append field `PublisReady` and remove `State` from edit attrs.
res.OverrideEditAttrs(func() {
  res.EditAttrs(res.EditAttrs(), "PublishReady", "-State")
})
```

## Metas

### Reconfigure Meta

QOR Admin will combine your Meta configurations, latest configuration will overwrite pervious one.

```go
user.Meta(&admin.Meta{Name: "Gender", Label: "Select Gender", Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female", "Unknown"}}})
user.Meta(&admin.Meta{Name: "Gender", Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female"}}})

// becomes

user.Meta(&admin.Meta{Name: "Gender", Label: "Select Gender", Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female"}}})
```

### Register Meta Processor

Meta Processor will be call each time reconfigure a `Meta`

```go
genderMeta := user.Meta(&admin.Meta{Name: "Gender", Label: "Select Gender", Config: &admin.SelectOneConfig{Collection: []string{"Male", "Female"}}})

genderMeta.AddProcessor(*admin.MetaProcessor{
  Name: "make-sure-label-is-select-gender",
  Handler: func(meta *admin.Meta) {
    meta.Label = "Select Gender"
  },
})
```

### Create New Meta Types

### Create Meta Config

### RegisterMetaConfigor

## Customize View

Checkout [customize templates](/admin/theming_and_customization.md#customize-templates) for how to overwrite view

### Register FuncMap

```go
Admin.RegisterFuncMap("my_fancy_func", func() string {
  return "my_fancy_func"
})
```

## View Actions

Load view actions for index/edit/new/show pages

## Router

Register new router into Admin
