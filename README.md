# Golangの害悪パッケージ

Golangで†いたずら†をできるパッケージたち。

> :warning: **決してこのパッケージを悪用しないでください。**OSSに対してこのパッケージを含むPRを出すようなことはやめましょう。

## Description
Golangは予約語が25個と少ない言語です。
その代わり、他の言語では予約語となっていることの多い`true`、`false`等もuniverse blockで定義されたPredicrared Identifierとなっています。
そのため、`true`や`false`などの名前の変数を定義し、元の`true`、`false`などを使えなくできるのは広く知られています。
これがファイル内に`true`、`false`などの文字列がない状態でされたら地獄が生まれそう、
と考え、実際にやってみたのがこのrepositoryです。

packageはfile blockで宣言されるIdentifierなため、packageでPredicrared Identifierを上書きすることが出来ます。
また、importは

## Install
```sh
go get github.com/mazrean/evil-package
```

## Usage
重要なことなのでもう一度。
> :warning: **決してこのパッケージを悪用しないでください。**OSSに対してこのパッケージを含むPRを出すようなことはやめましょう。

### Step 1
このリポジトリ内の好きなパッケージをimportしましょう。
ファイル内で`true`、`nil`などの事前定義された定数が使えなくなったりして地獄が生まれるでしょう。

#### Example
```go
package main

import (
  "fmt"

  "github.com/mazrean/evil-package/echo"
)

func main() {
  println(true) //error!
}
```

### Step 2
`evil-package`という名前では悪さをすることがすぐに見抜けれてしまうでしょう。
[replace](https://golang.org/ref/mod#go-mod-file-replace)を使って、
まともなパッケージに偽装しましょう。
より酷い地獄が生まれるかも知れません。

#### Example
```text:go.mod
module your.repository

go 1.15

require github.com/labstack/echo/v4 v4.1.14
replace github.com/labstack/echo/v4 v4.1.14 => github.com/mazrean/evil-package/echo
```

### Step 3
ここまでで十分害悪ですね。
しかし、これではPRをみられたときに`go.mod`に不自然な変更があると怪しまれてしまうかも知れません。
木を隠すなら森の中！というわけでpackageの更新をして、redirectに気づきづらくしましょう。
これでpackageの更新をしただけのように見えて、実はアプリケーションを破壊するPRの完成です。
