# Hidden / Readonly

## Hidden

You can hide a field from the form using the `hidden` Meta.

```
product.Meta(&admin.Meta{Name: "Name", Type: "hidden"})
```

## Readonly

To make a file read-only in a form, use the `readonly` Meta.

```
product.Meta(&admin.Meta{Name: "Name", Type: "readonly"})
```

{% include "/admin/common_meta_types_with_title.md" %}
