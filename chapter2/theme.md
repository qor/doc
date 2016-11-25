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

It is possible to completely overwrite [QOR Admin](../chapter2/setup.md) interface by specify another layout in a new theme. Continue use `fancy` as an example. You can define `{current path}/app/views/qor/themes/fancy/layout.tmpl` to overwrite default layout. But a complete new layout will lose most of the built-in feature of [QOR Admin](../chapter2/setup.md). So except for very special situation, we would recommend you to copy the default layout from [here](https://github.com/qor/admin/blob/master/views/layout.tmpl) and make your own layout based on it.

### Customizing Views {#customize-views}

When defined a resource in [QOR Admin](../chapter2/setup.md), a set of templates will be generated for the resource too.

For example, assume you have defined `Admin.AddResource(&Product{})`, You can overwrite the predefined templates in this directory.

- {current path}/app/views/qor/products/
  - index.tmpl
  - show.tmpl
  - edit.tmpl
  - new.tmpl

The default view path is `{current path}/app/views/qor`. If you want to customize your views from other places, you could register new paths by `admin.RegisterViewPath`.
