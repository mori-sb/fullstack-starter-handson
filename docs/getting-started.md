# Getting Started

このページは、ハンズオンに参加する人が最初に読むページです。

アプリ開発に慣れていなくても大丈夫です。
このハンズオンでは、最初からすべてを暗記したり、完璧に理解したりする必要はありません。

まずは、次の3つを目指します。

```text
1. 画面、API、DBの役割が分かる
2. コードがどの役割のものか見分けられる
3. 小さな変更を自分で試せる
```

## このハンズオンで作るもの

グルメ管理アプリを作ります。

行きたいお店や行ったお店を登録し、地域・ジャンル・ステータスで探せるアプリです。

できるようにすること:

- お店を登録する
- お店一覧を見る
- お店を編集する
- お店を削除する
- 地域で絞り込む
- ジャンルで絞り込む
- ステータスで絞り込む
- 画像URLを使って画像を表示する

## 最初に覚える全体像

まずは、この流れだけ覚えます。

```text
React
  画面を表示する
  入力を受け取る
  APIを呼ぶ

Spring Boot
  APIを受け取る
  処理する
  DBとやり取りする

DB
  データを保存する
```

ReactはDBを直接触りません。
ReactはSpring BootのAPIを呼びます。
Spring BootがDBとやり取りします。

## 最初に覚えるSpring Boot

Spring Bootは、役割ごとにコードを分けます。

まずはこの3つを覚えます。

```text
Controller  APIの入口
Service     処理を書く場所
Repository  DBとやり取りする場所
```

最初はこの流れだけで大丈夫です。

```text
React -> Controller -> Service -> Repository -> DB
```

DTO、Entity、Mapperは後から出てきます。
いきなり全部覚えようとしなくて大丈夫です。

Spring Boot側のディレクトリは、最終的に次のような形にします。

```text
backend/
└─ src/
   └─ main/
      ├─ java/
      │  └─ com/example/gourmet/
      │     ├─ GourmetApplication.java
      │     ├─ restaurant/
      │     │  ├─ RestaurantController.java
      │     │  ├─ RestaurantService.java
      │     │  ├─ RestaurantRepository.java
      │     │  ├─ Restaurant.java
      │     │  ├─ RestaurantRequest.java
      │     │  ├─ RestaurantResponse.java
      │     │  ├─ RestaurantMapper.java
      │     │  └─ RestaurantStatus.java
      │     └─ config/
      │        └─ WebConfig.java
      └─ resources/
         └─ application.yml
```

最初に見る場所:

```text
RestaurantController.java
RestaurantService.java
RestaurantRepository.java
```

慣れてきたら見る場所:

```text
Restaurant.java
RestaurantRequest.java
RestaurantResponse.java
RestaurantMapper.java
```

## 最初に覚えるReact

Reactは、画面を部品に分けて作ります。

Reactを読む前に、HTML、CSS、JavaScriptとの関係を押さえておきます。

```text
HTML        画面の構造を作る
CSS         見た目を整える
JavaScript  動きをつける
React       画面を部品として作り、データに応じて更新する
Tailwind CSS 見た目の指定をclassNameに書く
```

Reactでは、JSXという書き方を使って、JavaScriptの中にHTMLに近い見た目のコードを書きます。
今回はTailwind CSSを使うため、見た目の指定も `className` としてコンポーネントの中に書きます。

```jsx
<button className="rounded bg-blue-600 px-4 py-2 text-white">
  登録する
</button>
```

このコードでは、次の要素がまとまっています。

```text
button      HTMLに近い画面構造
className   Tailwind CSSによる見た目
登録する     画面に表示する文字
```

今回のアプリでは、次のように分けます。

```text
App
  RestaurantFilter
  RestaurantForm
  RestaurantList
    RestaurantCard
```

まずはこの2つを覚えます。

```text
props  親から子へ渡すデータ
state  画面の中で変わるデータ
```

React側のディレクトリは、最終的に次のような形にします。

```text
frontend/
└─ src/
   ├─ App.jsx
   ├─ main.jsx
   ├─ api/
   │  └─ restaurants.js
   ├─ components/
   │  ├─ RestaurantFilter.jsx
   │  ├─ RestaurantForm.jsx
   │  ├─ RestaurantList.jsx
   │  └─ RestaurantCard.jsx
   └─ styles/
      └─ app.css
```

最初に見る場所:

```text
App.jsx
components/RestaurantCard.jsx
components/RestaurantList.jsx
```

API連携で見る場所:

```text
api/restaurants.js
```

## 分からなくなったときの見方

分からなくなったら、まず「どこの話か」を考えます。

```text
画面の見た目や入力の話
  -> React

URLやJSONやAPIの話
  -> Controller

登録、編集、削除などの処理の話
  -> Service

DB保存や検索の話
  -> Repository / DB
```

全部を一度に理解しようとしないで、今見ているコードがどの役割なのかを確認します。

## エラーが出たとき

エラーは普通に出ます。
エラーが出ること自体は失敗ではありません。

まず次の順番で確認します。

```text
1. ブラウザのConsoleを見る
2. ブラウザのNetworkを見る
3. Spring Bootのログを見る
4. APIを単体で確認する
5. DBにデータがあるか確認する
```

特に、ReactとSpring BootをつなぐときはNetworkを見るのが大事です。

見るポイント:

- APIは呼ばれているか
- URLは正しいか
- ステータスコードは何か
- レスポンスのJSONは想定通りか

## AIを使うとき

AIは使って大丈夫です。

ただし、AIに作ってもらったコードをそのまま流さず、次のことを確認します。

- このファイルは何の役割か
- このAPIはどの画面から呼ばれるか
- このデータはReact、Spring Boot、DBのどこにあるか
- 変更したいとき、どのファイルを直すか

AIは実装を速くしてくれます。
でも、実務で大事なのは「何が作られたかを読めること」です。

## 今日できればOKのライン

毎日、全部を完璧に理解しなくて大丈夫です。

最低限、次の質問に答えられればOKです。

```text
今日作ったものは何ですか
どのファイルを触りましたか
画面、API、DBのどこに関係しますか
動作確認はどうやりましたか
1つ変えるならどこを変えますか
```

この5つを毎日確認します。
