## 退屈なことはPowerShellにやらせてもいいか

2019-07-26

CLR/H #109

Hidetsugu TAKAHASHI a.k.a manzyun

---

### はじめに - ご容赦ください

+ 就業の**合間**クオリティです。
+ 「軽い気持ちでPowerShellで爪楊枝未満ツール作ろう」がモットーです。
  + 凝った話はしないはず
  + PowerShellスクリプトの書き方は各自で

---

### スピーカー紹介

高橋秀羅（たかはし　ひでつぐ）

+ しがない放浪（派遣）プログラマ
+ C♯, VB.net, PHP, Python, JavaScript, Go とか
+ 2019年にもなって、作れるWebページは2000年代なデザイン
+ [CoderDojo](https://coderdojo.jp)の駄メンター

インターネッツでは「まんじゅ（́ん｀）」というふざけた名前で出没。

---

### PowerShellとは？

+ Windowsに *限らず* 使えるシェル。
+ しかしWindowsや.NETのオブジェクトの問い合わせに対しては *とてつもなく優秀*。
+ 後でもっと詳しい話があると思います。

---

### もう少しほめる

+ 一部のコマンドが、Unix, Linuxでのコマンドでも呼び出せる。
+ Windowsや.NETのオブジェクトには*とてつもなく*使える。
+ 作法は違うものの、Linuxで鍛えたCLIオペレーションの感覚がPowerShellにも応用ができる。

---

### CLIオペレーションの基本の「キ」

+ Windowsエクスプローラーで見ているディレクトリと、CLIで見ているディレクトリは違います
  + エクスプローラーで開いているところに移動するには、CLIでコマンドを入れて移動しなければいけない
+ 何かコマンドを入れてあげないと、何も返事をしてくれません
  + フォルダの中を見るのにもコマンド入力

---

# Demoる？

---

### 「CLIオペレーションの何がいいの？」

**パイプライン処理を組める**

---

### 「パイプライン処理が組めると何がいいの？」

**コマンドからコマンドにデータのバケツリレーができる**

---

### 「データのバケツリレーができると何がいいの？」

スクリプトが組める。

（間違えた表現をあえてすると）**マクロが組める。**

---

### 「で、CLIオペレーションの何がいいの？」

マウスポチポチのWindowsでの作業が**自動化**できる。

---

## 退屈なことはPowerShellにやらせてもいいか

2019-07-26

CLR/H #109

Hidetsugu TAKAHASHI a.k.a manzyun

---

### マウスポチポチ作業の怖いこと

+ 作業ミスが発生する
+ 同じ作業を単調にやるので飽きる
+ 最悪、多大な時間をかけて腱鞘炎などのリスクを高めることになる

と、ちょっと大げさに言いました

---

### PowerShellの基本的な使い方

+ 一番大事なヘルプの見方
+ フォルダ移動の方法
+ パイプライン処理
+ タブキー

---

#### 一番大事なヘルプの見方

+ `Get-Help`: ヘルプを見る
+ `Get-Help <コマンド>`: コマンドのヘルプを見る

---

#### フォルダ移動の方法

+ `Get-ChildItem`: 今いるフォルダの内容の問い合わせ
+ `Get-Location` : 今いるフォルダの問い合わせ
+ `Set-Location`: フォルダ移動
  + `\` でフォルダ区切り
  + `.\` 今いるフォルダへ
  + `..\` 一つ上のフォルダへ
+ `Get-Content`: ファイルの中身を問い合わせ

---

#### フォルダ移動の例

```powershell
PS C:\Users\User> Get-Location

Path
----
C:\Users\User

PS C:\Users\User> Get-ChildItem

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2019/07/22     15:23                .atom
d-----       2019/07/05      9:41                .config

            :
            :
            :
```

---

#### フォルダ移動の例

```powershell
PS C:\Users\User> Set-Location Documents
PS C:\Users\User> Get-ChildItem

# ...フォルダ内容問い合わせ結果、略...

PS C:\Users\User\Documents> Get-Content my_knowleadge.md

# ...ファイル内容問い合わせ結果、略...
```

---

### パイプライン処理

データのバケツリレー

+ `|`: 単純に左から右へ
+ `| ForEach`: 左のオブジェクトの結果の一つ一つに対して右の処理を行う
+ `| Where`: 左のオブジェクトの結果のうちから、右の式に該当したものを、更に右の処理へ渡す
+ `| Sort`: 左のオブジェクトの結果を、右の引数を基準に整列する。
+ `| Group`: 左のオブジェクトの結果から、右の引数を基準にして集計し、まとめる。

---

#### パイプライン処理の例

あるフォルダの中で、`Visual Studio` が頭に付くフォルダやファイルを取得し、名前の降順で並べる。

```powershell
Get-ChildItem | 
Where { $_.Name -like "Visual Studio*" } |
Sort -Property Name -Descending
```

---

#### パイプライン処理の例

```powershell

# ...結果前略

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       2019/07/23     11:05                Visual Studio 2015
d-----       2019/06/13      9:49                Visual Studio 2013
d-----       2019/06/25     13:57                Visual Studio 2010
d-----       2019/04/26     18:14                Visual Studio 2008
```

---

### PowerShellのあんな使い方、こんな使い方

+ 割と普通と思われる使い方
+ 昔の業務でやったやつに似たやつ
+ 手前味噌の業務中に作ったやつに似たやつ

---

### 割と普通と思われる使い方

商品の在庫状況を書いた、列名を書いた行も入っているShift-JISエンコードのCSVファイル `stock.csv` があるが、中身は整列されていない。

このCSVファイルの内容を、在庫の少ないものから上から順番（昇順）に並べたい。

---

#### 割と普通と思われる使い方

```powershell
$stock_data = Import-Csv .\stock.csv -Encoding Default
$stock_data | Sort "残り在庫"

# もしくは以下も可能
Import-Csv .\stock.csv -Encoding Default | Sort "残り在庫"
```

---

### 手前味噌の業務中に作ったやつに似たやつ

`packets.xml` というXMLファイルの中に、`packets` をルート要素とし、 `base64` という要素がいくつかある。この要素の中身をデシリアライズする。
（一部要求をぼかしています）

---

#### 手前味噌の業務中に作ったやつに似たやつ

制作にかけた時間、4時間。短縮された業務時間、24時間以上。

```powershell
[xml](Get-Content packets.xml).base64 |
Foreach {[System.Text.Encoding]::Default.Getstring(System.Convert]::FromBase54String($_)}
```

---

### 言いたかったこと

+ 2019年ですが……。
  + CLIの力は素晴らしいぞ！
+ PowerShellの感覚がLINQにも活かせる？
+ 知って使えて損はない。
+ みんなも爪楊枝未満ツールをいっぱい作ろう

---

### 参考資料

+ [PowerShell スクリプティング - Microsoft](https://docs.microsoft.com/ja-jp/powershell/scripting/overview)
+ [PowerShellの演算子一覧 - しばたテックブログ](https://blog.shibata.tech/entry/2015/12/03/000000)

---

### Thank's for listening

+ このスライドは[Git Pitch](https://gitpitch.com/)で作成しました。
+ 再配布は [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.ja)に基づいて許可いたします。
