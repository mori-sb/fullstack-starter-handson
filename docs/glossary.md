# Glossary

ハンズオンで使う用語集です。

最初から全部覚える必要はありません。
分からない言葉が出てきたときに戻って確認します。

## Webアプリ全体

### フロントエンド

ユーザーが見る画面側のこと。

この教材ではReactで作る。

### バックエンド

画面の裏側で処理する側のこと。

この教材ではSpring Bootで作る。

### API

フロントエンドとバックエンドの窓口。

ReactはAPIを呼び、Spring BootはAPIを受け取る。

### JSON

ReactとSpring Bootの間でデータをやり取りする形式。

```json
{
  "name": "Cafe Sakura",
  "area": "新宿"
}
```

### HTTPメソッド

APIで何をしたいかを表すもの。

```text
GET     取得
POST    登録
PUT     更新
DELETE  削除
```

## Spring Boot

### Class

Javaで処理やデータのまとまりを定義するもの。

例:

- `MovieController`
- `MovieService`
- `RestaurantController`

### DI

Dependency Injectionの略。

必要な部品を自分で `new` するのではなく、Spring Bootに渡してもらう仕組み。

例:

```java
public MovieController(MovieService movieService) {
    this.movieService = movieService;
}
```

この例では、`MovieController` が使う `MovieService` をSpring Bootが渡している。

### Bean

Spring Bootが管理している部品のこと。

`@RestController` や `@Service` などを付けたclassは、Spring Bootに見つけてもらいやすくなる。

### Controller

APIの入口。

URLとHTTPメソッドを受け取り、Serviceを呼ぶ。

### Service

アプリの処理を書く場所。

登録、編集、削除などの中心になる。

### Repository

DBとやり取りする場所。

保存、取得、削除などを担当する。

### Entity

DBに保存するデータの形。

### DTO

APIで受け渡しするデータの形。

Request DTOはReactから受け取る形。
Response DTOはReactへ返す形。

### Mapper

DTOとEntityを変換するもの。

### Validation

入力値が正しいか確認すること。

例:

- 店名が空ではない
- ステータスが正しい値である

## React

### Component

画面を作る部品。

例:

- RestaurantForm
- RestaurantList
- RestaurantCard

### props

親コンポーネントから子コンポーネントへ渡すデータ。

### state

画面の中で変わるデータ。

### useState

stateを使うためのReactの機能。

### useEffect

画面表示時や値の変化時に処理を実行するためのReactの機能。

### fetch

JavaScriptからAPIを呼ぶための機能。

## 開発・確認

### CORS

ブラウザの安全機能。

ReactとSpring BootのURLが違うときに、通信が止められることがある。

### Console

ブラウザでJavaScriptエラーを見る場所。

### Network

ブラウザでAPI通信を見る場所。

APIのURL、ステータスコード、レスポンスを確認できる。
