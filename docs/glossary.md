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

## Java

### class

Javaで処理やデータのまとまりを定義するもの。

例:

```java
public class MovieService {
}
```

### method

classの中に書く処理。

例:

```java
public List<MovieResponse> findAll() {
    return List.of();
}
```

### field

classが持つ値。

例:

```java
private final MovieService movieService;
```

### constructor

classを作るときに呼ばれる入口。
Spring Bootでは、DIでServiceなどを受け取るときによく使う。

例:

```java
public MovieController(MovieService movieService) {
    this.movieService = movieService;
}
```

### record

値をまとめて持つためのJavaの書き方。
この教材では、Response DTOやRequest DTOで使う。

### List

複数件のデータを扱う入れ物。

`List<MovieResponse>` は、`MovieResponse` が複数件入るという意味。

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

### DBモデル

DBテーブルの1行をJavaで受け取るための形。

この教材ではMyBatisを使うため、JPAの `@Entity` は扱わない。

DBモデルは必ず必要なものではない。
小さいAPIではDTOだけで実装することもある。

この教材では、APIで受け渡しする形とDBの1行を扱う形を分けて考えるためにDBモデルを使う。

### DTO

APIで受け渡しするデータの形。

Request DTOはReactから受け取る形。
Response DTOはReactへ返す形。

### MyBatisのMapper

MyBatisでSQLを書くRepositoryに付ける目印。

```java
@Mapper
public interface RestaurantRepository {
}
```

DTOとDBモデルの変換は、この教材ではServiceの中で行う。

### Migration

DBのテーブル作成や変更を、SQLファイルとして履歴管理する仕組み。

この教材ではFlywayを使い、`backend/src/main/resources/db/migration/` にSQLファイルを置く。

例:

```text
V1__create_movies_table.sql
```

Spring Bootを起動すると、Flywayがまだ実行されていないmigrationをDBへ反映する。

### Schema

DBの構造。

どんなテーブルがあり、どんなカラムを持つかを表す。

### Validation

入力値が正しいか確認すること。

例:

- 店名が空ではない
- ステータスが正しい値である

## JavaScript / TypeScript

### const

値を入れる変数を定義する。

```jsx
const name = "Cafe Sakura";
```

### function

処理をまとめる。

```jsx
function handleClick() {
  console.log("clicked");
}
```

### object

複数の値を名前付きでまとめる。

```jsx
const restaurant = {
  name: "Cafe Sakura",
  area: "新宿",
};
```

### array

複数件のデータを並べたもの。

```jsx
const restaurants = [
  { id: 1, name: "Cafe Sakura" },
  { id: 2, name: "Ginza Kitchen" },
];
```

### map

配列を1件ずつ取り出して、別の形に変換する。
Reactでは、配列を画面表示に変換するときによく使う。

### type

値の形を表す考え方。

TypeScriptでは、`string`、`number`、`boolean`、配列、オブジェクトの形などを扱う。

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
