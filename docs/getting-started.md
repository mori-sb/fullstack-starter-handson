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

## 事前に準備するもの

このハンズオンでは、次のツールを使います。

```text
IntelliJ IDEA       Spring Bootのコードを書く
VS Code             Reactのコードを書く
Rancher Desktop     DBコンテナを起動する
Bruno               APIの動作確認をする
Git / GitHub        コードと資料を管理する
Java                Spring Bootを動かす
Node.js / npm       Reactを動かす
```

当日は、すべての設定が終わっている前提にはしません。
ただし、最初に環境確認をしておくと、実装中の詰まりを減らせます。

## 環境確認

ターミナルで次のコマンドを実行します。

```bash
java -version
node -v
npm -v
git --version
```

確認できればよいこと:

```text
Javaのバージョンが表示される
Node.jsのバージョンが表示される
npmのバージョンが表示される
Gitのバージョンが表示される
```

コマンドが見つからない場合は、実装に入る前にツールのインストールまたはPATH設定を確認します。

## Brunoの準備

Brunoは、React画面を作る前にAPIを直接確認するために使います。
このハンズオンでは、APIの動作確認はBrunoで行います。

確認すること:

- Brunoを起動できる
- WorkspaceまたはCollectionを作成できる
- 新しいリクエストを作成できる
- HTTPメソッドを `GET`、`POST` などに変更できる
- URLに `http://localhost:8080/api/restaurants` を入力できる
- API実行後にステータスコードとJSONレスポンスを確認できる

最初に作っておくリクエスト:

```text
Name: Get restaurants
Method: GET
URL: http://localhost:8080/api/restaurants
```

POSTを確認するときは、BodyをJSONにして送ります。

```text
Name: Create restaurant
Method: POST
URL: http://localhost:8080/api/restaurants
Body: JSON
```

Bodyの例:

```json
{
  "name": "Cafe Sakura",
  "area": "新宿",
  "genre": "カフェ",
  "memo": "落ち着いて作業できそう",
  "imageUrl": "https://example.com/cafe.jpg",
  "status": "WANT_TO_GO"
}
```

Brunoで確認する流れ:

```text
1. Spring Bootを起動する
2. BrunoでGETリクエストを作る
3. URLに http://localhost:8080/api/restaurants を入れる
4. Sendを押す
5. JSONが返ることを確認する
```

ReactからAPIを呼ぶ前にBrunoで確認すると、問題がReact側なのかAPI側なのか切り分けやすくなります。

## IntelliJ IDEAの初期設定

IntelliJ IDEAは、Spring Boot側の開発に使います。

確認すること:

- プロジェクトとして `backend/` を開ける
- Java SDKが設定されている
- GradleまたはMavenの読み込みが完了している
- `GourmetApplication.java` を実行できる
- 実行ログにエラーが出ていない

最初に見る場所:

```text
backend/src/main/java/.../GourmetApplication.java
backend/src/main/resources/application.yml
```

Spring Bootが起動できたら、API側の準備は最初の段階としてOKです。

## VS Codeの初期設定

VS Codeは、React側の開発に使います。

確認すること:

- プロジェクトとして `frontend/` を開ける
- ターミナルで `npm install` を実行できる
- `npm run dev` でReact開発サーバーを起動できる
- ブラウザで `http://localhost:5173` を開ける

最初に見る場所:

```text
frontend/src/App.jsx
frontend/src/components/
frontend/src/api/
```

Reactが起動できたら、画面側の準備は最初の段階としてOKです。

## Rancher DesktopでDBを起動する

DBはRancher Desktopを使ってコンテナで起動します。

確認すること:

- Rancher Desktopが起動している
- コンテナエンジンが使える
- ターミナルで `docker --version` が表示される
- DBコンテナを起動できる

DB起動は、リポジトリに `compose.yml` または `docker-compose.yml` がある場合はそれを使います。

```bash
docker compose up -d
```

起動後に確認すること:

```bash
docker ps
```

見るポイント:

- DBコンテナが起動している
- `STATUS` が `Up` になっている
- Spring Bootの `application.yml` に書いたDB接続先と合っている

DB接続で詰まった場合は、ReactではなくSpring BootとDBの設定を確認します。

## Gitとブランチ

作業は `develop` ブランチで進めます。

```bash
git branch
git status
```

確認すること:

```text
今いるブランチがdevelopである
作業前に不要な差分がない
変更したファイルを把握している
```

作業後は、差分を確認してからコミットします。

```bash
git status
git diff
git add .
git commit -m "作業内容が分かるメッセージ"
```

このハンズオンでは、Git操作そのものを深く扱いすぎません。
ただし「今どのファイルを変更したか」は毎回確認します。

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
      │     ├─ controller/
      │     │  ├─ MovieController.java
      │     │  └─ RestaurantController.java
      │     ├─ service/
      │     │  ├─ MovieService.java
      │     │  └─ RestaurantService.java
      │     ├─ repository/
      │     │  ├─ MovieRepository.java
      │     │  └─ RestaurantRepository.java
      │     ├─ entity/
      │     │  ├─ Movie.java
      │     │  ├─ Restaurant.java
      │     │  └─ RestaurantStatus.java
      │     ├─ dto/
      │     │  ├─ movie/
      │     │  │  ├─ MovieRequest.java
      │     │  │  └─ MovieResponse.java
      │     │  └─ restaurant/
      │     │     ├─ RestaurantRequest.java
      │     │     └─ RestaurantResponse.java
      │     ├─ mapper/
      │     │  ├─ MovieMapper.java
      │     │  └─ RestaurantMapper.java
      │     └─ config/
      │        └─ WebConfig.java
      └─ resources/
         └─ application.yml
```

この教材では、役割が見つけやすいように層ごとにディレクトリを分けます。
`controller/` にはController、`service/` にはService、`repository/` にはRepositoryを置きます。

最初に見る場所:

```text
controller/RestaurantController.java
service/RestaurantService.java
repository/RestaurantRepository.java
```

慣れてきたら見る場所:

```text
entity/Restaurant.java
dto/restaurant/RestaurantRequest.java
dto/restaurant/RestaurantResponse.java
mapper/RestaurantMapper.java
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

## AIへの指示の出し方

AIに依頼するときは、次の4つを入れます。

```text
1. 何を作りたいか
2. どの技術で作るか
3. どのファイルに書くか
4. 完成後に何を確認したいか
```

悪い例:

```text
Reactでいい感じに作って
```

この指示だと、どのファイルに何を作るべきかが曖昧です。

良い例:

```text
Reactでグルメ管理アプリのお店カードを作ってください。
ファイルは frontend/src/components/RestaurantCard.jsx です。
propsとして restaurant を受け取り、name、area、genre、memo、imageUrl、status を表示してください。
Tailwind CSSを使って、画像付きカードとして見やすくしてください。
作成後に、このコンポーネントがどのpropsを使っているか説明してください。
```

Spring Bootの例:

```text
Spring Bootでお店一覧APIを作ってください。
Controller、Service、Repositoryの役割を分けてください。
APIは GET /api/restaurants です。
レスポンスは id、name、area、genre、memo、imageUrl、status を含むJSON配列にしてください。
作成後に、リクエストがController、Service、Repositoryをどの順番で通るか説明してください。
```

AIに実装を依頼した後は、必ず次の確認をします。

- どのファイルが作られたか
- どのファイルが変更されたか
- 画面、API、DBのどこを担当するコードか
- 動作確認の方法は何か
- 不要に複雑な実装になっていないか

## 自分で進めるときの基本サイクル

演習では、次の流れで進めます。

```text
1. 資料で今日のゴールを確認する
2. 触るファイルを確認する
3. AIに小さく依頼する
4. 生成されたコードを読む
5. 動かして確認する
6. エラーが出たら確認順序に沿って切り分ける
7. 分かったことをメモする
```

AIへの依頼は小さく分けます。

```text
悪い進め方:
グルメ管理アプリを全部作って

良い進め方:
まず RestaurantCard を作る
次に RestaurantList を作る
次に RestaurantForm を作る
次に API接続を作る
```

小さく依頼すると、生成されたコードを確認しやすくなります。

## ライブ実装と演習の分け方

ハンズオンでは、完成アプリを最初から全部見せることはしません。
まず最小の流れを一緒に確認し、残りを演習で完成させます。

ライブ実装では `Movie` を題材にします。
演習では `Restaurant` を題材にします。

```text
Movieで見た構造を、Restaurantに置き換えて実装する
```

```text
一緒に確認すること:
画面 -> API -> DB の基本の流れ

演習で完成させること:
編集、削除、フィルタ、エラー表示など
```

AIを使うときも、完成アプリを一括生成するのではなく、今取り組んでいる機能だけを依頼します。

```text
良い依頼:
RestaurantCardだけ作ってください
登録APIだけ作ってください
地域フィルタだけ追加してください

避けたい依頼:
グルメ管理アプリを全部完成させてください
```

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
