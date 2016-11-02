# Search

It is possible to specify database table columns as search attributes by using `SearchAttrs`, the columns will be used to perform search queries.

## Examples

```go
// Search products with its name, code, category's name, brand's name
product.SearchAttrs("Name", "Code", "Category.Name", "Brand.Name")
```

If you want to fully customize the search function, you can set the `SearchHandler`:

```go
order.SearchHandler = func(keyword string, context *qor.Context) *gorm.DB {
  // search orders
}
```

## Searching by nested relations

It is also possible to specify nested relations.

```go
// Search order by user's name and email. shipping address's contact name, addresses.
order.SearchAttrs("User.Name", "User.Email", "ShippingAddress.ContactName", "ShippingAddress.Address1", "ShippingAddress.Address2")
```

## Search Center

You might want to search a broad range of resources from a single web page, in this case `Search Center` is for you!  Simply add resources that you want to be searchable to the [QOR Admin](../chapter2/setup.md) value's search center:

```go
// add resource `product`, `user`, `order` to search resources
Admin.AddSearchResource(product, user, order)
```

[Search Center Online Demo](http://demo.getqor.com/admin/!search)
