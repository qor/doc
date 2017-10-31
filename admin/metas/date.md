# Datetime / Date

## Datetime

For the `time.Time` attribute, QOR generates a date & time selector automatically.

```
type Product struct {
  gorm.Model
  Name        string
  ReleaseDate time.Time
}
```

The "ReleaseDate" in the form will look like this in QOR Admin:

![Datetime](datetime.png)

## Date

We save both Date and Datetime as `time.Time` in the database. But we support `Date` as its own field type. So if we change above example to:

```
p.Meta(&admin.Meta{Name: "ReleaseDate", Type: "date"})
```

The "ReleaseDate" field will be purely a date selector (i.e. no time entry provided).

{% include "/admin/common_meta_types_with_title.md" %}
