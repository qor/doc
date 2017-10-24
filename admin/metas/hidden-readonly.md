# Hidden / Readonly

## Hidden

You can hide a field from the form using the `Hidden` Meta.

```
product.Meta(&admin.Meta{Name: "Name", Type: "Hidden"})
```

## Readonly

To make a file read-only in a form, use the `Readonly` Meta.

```
product.Meta(&admin.Meta{Name: "Name", Type: "Readonly"})
```

{% include "/admin/common_meta_types_with_title.md" %}
