# Theming & Customization

## Global Stylesheet / Javascript

QOR Admin will auto-load javascripts and stylesheet files based on your Admin's [SiteName](/admin/general.md#sitename)

For example, say you set the site name as `Qor Demo`, QOR Admin will look up `{qor view paths}/assets/javascripts/qor_demo.js` and `{qor view paths}/assets/stylesheets/qor_demo.css`, and load them if present.

## View Paths

When QOR Admin looks up templates when rendering pages, it uses [AssetFS](/admin/general.md#assetfs).

A [default implemention](https://github.com/qor/assetfs/blob/master/filesystem.go) of `AssetFS` is looking up templates from filesystem from registered view paths.

## Themes

QOR Admin provides flexible template customization. You can define your own theme for a resource, overwrite a specific page of a resource, like the edit page of the user.

A custom theme for a [Resource](/admin/resources.md) in QOR Admin can be applied using a custom javascript and css file. To apply a custom theme, set the theme name using the `UseTheme` method, this will load `assets/javascripts/fancy.js` and `assets/stylesheets/fancy.css` from the theme path.

For example:

```go
product := Admin.AddResource(&Product{})
product.UseTheme("fancy")
```

This means `{qor view paths}/themes/fancy/assets/stylesheets/fancy.css` and `{qor view paths}/themes/fancy/assets/javascripts/fancy.js` will be loaded.

## Menus

[QOR Admin](/admin/README.md) provides flexible way to manage menus. By default, `Resource` will be listed at the top level of menu. You can set the position manually.

```go
Admin.AddResource(&User{})

Admin.AddResource(&Product{}, &admin.Config{Menu: []string{"Product Management"}})
Admin.AddResource(&Color{}, &admin.Config{Menu: []string{"Product Management"}})
Admin.AddResource(&Size{}, &admin.Config{Menu: []string{"Product Management"}})

Admin.AddResource(&Order{}, &admin.Config{Menu: []string{"Order Management"}})
```

This will generate menus like this:

![menu-demo](menu-demo.png)

If you don't want a resource to be displayed in the menu, pass the `Invisible` option:

```go
Admin.AddResource(&User{}, &admin.Config{Invisible: true})
```

### Register a menu with custom URL

You can add menu with custom URL, like this

```go
Admin.AddMenu(&admin.Menu{Name: "Dashboard", Link: "/admin"})

// Register nested menu, The "menu" under "Dashboard"
Admin.AddMenu(&admin.Menu{Name: "menu", Link: "/link", Ancestors: []string{"Dashboard"}})

// Register menu with permission, User has "admin" permission could access "Report" page.
Admin.AddMenu(&admin.Menu{Name: "Report", Link: "/admin", Permission: roles.Allow(roles.Read, "admin")})
```

Please check [Authentication](/admin/authentication.md#authorization-for-menus) for more permission control informations.

### Configure your own menu icon

[QOR Admin](/admin/README.md) uses icons from [material icons](https://material.io/icons/), and pre-generated attribute name on each menu. So it is very easy to customize your own menu icon.

Assume you are adding icon to `Product`. First, go to [material icons](https://material.io/icons/) page and pick the icon you wanted then copy the content in the screenshot

![menu-icon-demo](menu-icon-demo.png)

Then put it in the css, about how to customize css for QOR Admin go check [QOR Admin theme](#global-stylesheet--javascript).

```css
[qor-icon-name*="Products"] > a::before {
  content: "\E2BF";
}
```

Go to [QOR Admin](/admin/README.md), you should see the icon has been displayed beside the product menu.
