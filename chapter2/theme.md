# Theme and Customize views

[QOR](https://github.com/qor/qor) provides flexible template customization. You can define your own theme for a resource, overwrite a specific page of a resource, like the edit page of the user.

### Theme

A custom theme for a [Resource](../chapter2/resource-intro.md) in [QOR Admin](../chapter2/setup.md) can be applied using a custom javascript and css file. To apply a custom theme, set the theme name using the `UseTheme` method, this will load `assets/javascripts/fancy.js` and `assets/stylesheets/fancy.css` from the theme path.

For example:

```go
product := Admin.AddResource(&Product{})
product.UseTheme("fancy")
```

This means `{current path}/app/views/qor/themes/fancy/assets/stylesheets/fancy.css` and `{current path}/app/views/qor/themes/fancy/assets/javascripts/fancy.js` will be loaded.

#### Using different layout

It is possible to completely overwrite [QOR Admin](../chapter2/setup.md) interface by specify another layout in a new theme. Continue use `fancy` as an example. You can define `{current path}/app/views/qor/themes/fancy/layout.tmpl` to overwrite default layout. But a complete new layout will lose most of the built-in feature of [QOR Admin](../chapter2/setup.md). So except for very special situation, we would recommend you to copy the default layout from [here](https://github.com/qor/admin/blob/master/views/layout.tmpl) and make your own layout base on it.

### Customizing Views {#customize-views}

When defined a resource in [QOR Admin](../chapter2/setup.md), a set of templates will be generated for the resource too.

For example, assume you have defined `Admin.AddResource(&Product{})`, You can overwrite the predefined templates in this directory.

- {current path}/app/views/qor/products/
  - [index.tmpl](https://github.com/qor/admin/blob/master/views/index.tmpl)
  - [show.tmpl](https://github.com/qor/admin/blob/master/views/show.tmpl)
  - [edit.tmpl](https://github.com/qor/admin/blob/master/views/edit.tmpl)
  - [new.tmpl](https://github.com/qor/admin/blob/master/views/new.tmpl)

The default view path is `{current path}/app/views/qor`. If you want to customize your views from other places, you could register new paths by `admin.RegisterViewPath`.

If you only want to make changes on views rather than redefine views and controller/handler for a resource. We recommend you to copy codes from the default template(click the template name in above list to view the default template) and make changes base on it.

#### Available FuncMaps in the template

[QOR Admin](../chapter2/setup.md) has some predefined functions in template. You can use them in customized template too. Here's some common used functions, Check [FuncMaps](https://github.com/qor/admin/blob/master/func_map.go#L909) for the full list.

NOTE: Ignored wrapped brackets syntax of template in the example

| Name | Description | Example |
| --- | --- | --- |
| current_user | Get the current logged in user. | &nbsp; |
| has_create_permission | Determine whether current user has create permission on given resource | `if has_create_permission .Resource` |
| has_read_permission | Determine whether current user has read permission on given resource | `if has_read_permission .Resource` |
| has_update_permission | Determine whether current user has update permission on given resource | `if has_update_permission .Resource` |
| has_delete_permission | Determine whether current user has delete permission on given resource | `if has_delete_permission .Resource` |
| link_to | Generate a hyperlink | `link_to linktext, url`|
| logout_url | Generate logout url | `logout_url` |
| raw | Display raw html in the template | `raw "<div>enclosed in div</div>"` |
| stringify | Stringify the parameter | `stringify 123`|
| t | Short for `I18n.T`, to display translation in the template, check [I18n](../plugins/i18n.md) for detail. | `t "demo.greet" "Hello, {{$1}}" "John"` |
| url_for | Generate url for resource value | `url_for .Result`, Assume `.Result` is a product, the value looks like `/admin/products/1`|

If you need explanation to other functions in the FuncMaps, leave a message to us, we will update the list.
