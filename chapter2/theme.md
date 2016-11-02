# Theme and Customize views

#### Using a Theme

A custom theme for a [Resource](../chapter2/resource-intro.md) in [QOR Admin](../chapter2/setup.md) can be applied using a custom javascript and css file, for example to make a product page looks fancy. To apply a custom theme, provide the theme name using the `UseTheme` method, this will load `assets/javascripts/fancy.js` and `assets/stylesheets/fancy.css` from [QOR view paths](#customizing-views)

```go
product.UseTheme("fancy")
```

#### Customizing Views {#customize-views}

The default view path is

`{current path}/app/views/qor`

The default assets path is

- Javascripts `{current path}/app/views/qor/assets/javascripts`
- Stylesheets `{current path}/app/views/qor/assets/stylesheets`

[QOR Admin](../chapter2/setup.md) will look up templates in [QOR Admin](../chapter2/setup.md) view paths and use them to render any [QOR Admin](../chapter2/setup.md) page. By placing your own templates in `{current path}/app/views/qor` you can extend your application by customizing it's views.

If you want to customize your views from other places, you could register any new paths with `admin.RegisterViewPath`.

#### Customize Views Rules:

* To overwrite a template, create a file with the same name under `{current path}/app/views/qor`.
* To overwrite templates for a specific resource, put templates with the same name in `{qor view paths}/{resource param}`, for example `{current path}/app/views/qor/products/index.tmpl`.
* To overwrite templates for resources using a theme, put templates with the same name in `{qor view paths}/themes/{theme name}`.
