# BindataFS

[BindataFS](https://github.com/qor/bindatafs) will compile [QOR](https://github.com/qor/qor) templates into binary utilizing [go-bindata](https://github.com/jteeuwen/go-bindata)

[![GoDoc](https://godoc.org/github.com/qor/bindatafs?status.svg)](https://godoc.org/github.com/qor/bindatafs)

## Usage

Install [BindataFS](https://github.com/qor/bindatafs)

```sh
$ go install github.com/qor/bindatafs
```

To initialize [BindataFS](https://github.com/qor/bindatafs) for your project, set the path you want to store [BindataFS](https://github.com/qor/bindatafs) related files, e.g. `config/bindatafs`:

```sh
$ bindatafs config/bindatafs
```

And then to use [BindataFS](https://github.com/qor/bindatafs) with [QOR Admin](../chapter2/setup.md):

```go
import "<your_project>/config/bindatafs"

func main() {
  Admin = admin.New(&qor.Config{DB: db.Publish.DraftDB()})
  Admin.SetAssetFS(bindatafs.AssetFS)
}
```

Compiling [QOR](https://github.com/qor/qor) templates is as easy as:

```sh
go run main.go --compile-qor-templates
```

Finally, run with compiled templates:

```sh
go run -tags 'bindatafs' main.go
```

And, if need be, to run normally again:

```sh
go run main.go
```
