# BindataFS

[BindataFS](https://github.com/qor/bindatafs) compile [QOR](https://github.com/qor/qor) templates into binary with [go-bindata](https://github.com/jteeuwen/go-bindata)

[![GoDoc](https://godoc.org/github.com/qor/bindatafs?status.svg)](https://godoc.org/github.com/qor/bindatafs)

## Usage

Install [BindataFS](https://github.com/qor/bindatafs)

```sh
$ go install github.com/qor/bindatafs
```

Initialize [BindataFS](https://github.com/qor/bindatafs) for your project, `config/bindatafs` is the path you want to store [BindataFS](https://github.com/qor/bindatafs) related files

```sh
$ bindatafs config/bindatafs
```

Use [BindataFS](https://github.com/qor/bindatafs) for [QOR Admin](https://github.com/qor/admin)

```go
import "<your_project>/config/bindatafs"

func main() {
  Admin = admin.New(&qor.Config{DB: db.Publish.DraftDB()})
  Admin.SetAssetFS(bindatafs.AssetFS)
}
```

Compiling [QOR](https://github.com/qor/qor) templates

```sh
go run main.go --compile-qor-templates
```

Run with compiled templates

```sh
go run -tags 'bindatafs' main.go
```

Run normally

```sh
go run main.go
```
