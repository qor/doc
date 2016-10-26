# Datetime / Date

## Datetime

For the `time.Time` attribute, QOR generates date & time selector automatically.

For example

```
type Product struct {
  gorm.Model
  Name        string
  ReleaseDate time.Time
}
```

The "ReleaseDate" in form shall looks like

![Datetime](../metas/meta-datetime.png)

## Date

We save both Date and Datetime as `time.Time` in the database. But we support `Date` as a field type.

So if we change above example to

```
p.Meta(&admin.Meta{Name: "ReleaseDate", Type: "date"})
```

The "ReleaseDate" field will be a pure date selector.

