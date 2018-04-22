# Virtual Host [![GoDoc](https://godoc.org/github.com/go-mego/vhost?status.svg)](https://godoc.org/github.com/go-mego/vhost)

Virtual Host 套件能夠協助開發者在單個 Golang 程式上建立多個 Mego 實例並以不同網域名稱為差分進入點。

# 索引

* [安裝方式](#安裝方式)
* [使用方式](#使用方式)
    * [動態網域](#動態網域)

# 安裝方式

打開終端機並且透過 `go get` 安裝此套件即可。

```bash
$ go get github.com/go-mego/vhost
```

# 使用方式

透過 `vhost.New` 來建立新的虛擬主機，並且依照不同請求網址來呼叫不同的 Mego 實例。這很適合用在單台主機同時擁有開發、正式、測試伺服端。

```go
package main

import (
	"github.com/go-mego/mego"
	"github.com/go-mego/vhost"
)

func main() {
	m1 := mego.New()
	m2 := mego.New()
	// 這會依照請求網域決定應該使用哪個 Mego 實例。
	vhost.New("dev.yami.io", m1)
	vhost.New("api.yami.io", m2)
	vhost.Run()
}
```

## 動態網域

你能夠透過在網域中加上萬用符號（`*`）來允許動態子網域名稱，並且指向到同個 Mego 實例。此功能可讓你在路由中依照不同網域名稱進行不同的動作。

```go
func main() {
	m1 := mego.New()
	// 任何主網域為 `yami.io` 的都會使用 `m1` 實例。
	vhost.New("*.yami.io", m1)
	vhost.Run()
}
```