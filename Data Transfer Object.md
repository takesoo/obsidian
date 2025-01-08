---
tags:
  - コーディング
  - デザインパターン
aliases:
  - DTO
---
- 異なるプログラム間やコンピュータ間でひとまとまりのデータを受け渡す際に用いられる
- メンバ変数があるだけのクラスで、データ操作などのビジネスロジックやメソッドを持たない。
```go
// DTOにはデータ操作などのビジネスロジックやメソッドを持たない
type PersonDTO struct {
    FirstName string `json:"firstName"`
    LastName  string `json:"lastName"`
    Age       int    `json:"age"`
}

func main() {
    person := PersonDTO{
        FirstName: "John",
        LastName:  "Doe",
        Age:       30,
    }
    jsonData, err := json.Marshal(person) // DTOをJSON形式に変換して、別のレイヤーやシステムに転送できる
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(string(jsonData))
}

```
- メリット
	- 