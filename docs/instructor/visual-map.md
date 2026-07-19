# Visual Map

この教材では、文字だけで説明しない。

迷いやすい概念は、できるだけ図にする。
画像は多めでよい。
ただし、1枚に詰め込みすぎず、段階的に見せる。

## 画像を使う方針

- 1枚の画像で1つの概念だけ説明する
- 左から右に流れる図を基本にする
- 右に行くほどDBに近づく構成にする
- まず基本の流れを見せてから、DTO、DBモデル、MyBatisなどを追加する
- 画像を見せた後に、対応するコードを見る
- 画像の中の用語とコードのクラス名を対応させる

## Day 1: 全体像

### 1. グルメ管理アプリの完成画面モック

ファイル名:

```text
images/app-screen-mock.png
```

使う場面:

Day1の「画面の完成イメージ」。

伝えること:

- これから作るアプリの最終イメージ
- 行きたいお店、行ってよかったお店、お気に入りのお店をメモしておくサイトである
- 地域、ジャンル、ステータスで絞り込める
- お店カードに画像、店名、地域、ジャンル、メモ、ステータスが表示される
- 画面の情報とAPIのJSONが対応している

生成依頼文:

```text
モダンなSaaSアプリの画面モックとして、グルメ管理アプリの完成イメージを作ってください。

画面はWebアプリのダッシュボード風にしてください。
上部に「グルメ管理アプリ」というタイトルを置き、右上に「+ 登録する」ボタンを配置してください。
その下にフィルタ欄として「地域」「ジャンル」「ステータス」のセレクトボックスを横並びで配置してください。

メインにはお店カードを3枚並べてください。
各カードには、料理または店舗の画像、店名、地域 / ジャンル、ステータスバッジ、メモ、編集ボタン、削除ボタンを入れてください。

カード例:
1. Cafe Sakura / 新宿 / カフェ / 行きたい / 落ち着いて作業できそう
2. Ginza Kitchen / 銀座 / 洋食 / お気に入り / ランチがよかった
3. Shibuya Coffee / 渋谷 / カフェ / 行った / 作業しやすそう

白または薄いグレー背景、角丸カード、細い枠線、控えめな影、ネイビー・シアン・グリーンをアクセントにしてください。
実際の教材で最初に見せる完成画面なので、説明図ではなく、完成したアプリのスクリーンショット風にしてください。
文字は日本語で、読みやすく、情報が詰まりすぎないようにしてください。
```

### 2. Webアプリ全体構成図

ファイル名:

```text
images/app-overview.png
```

使う場面:

ハンズオンの最初。

伝えること:

- ユーザーはブラウザを使う
- Reactは画面を表示する
- Spring BootはAPIを処理する
- DBはデータを保存する
- ReactはDBを直接触らない

### 3. HTTPリクエスト / レスポンス図

ファイル名:

```text
images/http-json-flow.png
```

使う場面:

APIとは何かを説明するとき。

伝えること:

- ReactからSpring BootへHTTPリクエストが飛ぶ
- Spring BootからReactへJSONレスポンスが返る
- JSONは画面とAPIの間で受け渡すデータ

### 4. 画面操作とAPIの対応図

ファイル名:

```text
images/screen-api-map.png
```

伝えること:

- 一覧を見るときはGET
- 登録するときはPOST
- 編集するときはPUT
- 削除するときはDELETE

### 5. 画像URL表示の流れ

ファイル名:

```text
images/image-url-flow.png
```

伝えること:

- DBに保存するのは画像ファイルではなく画像URL
- APIはimageUrlをJSONで返す
- Reactは`img`タグで画像を表示する

## Day 2: Spring Bootの基本

### 5. Spring Bootディレクトリ構造図

ファイル名:

```text
images/spring-directory-structure.png
```

伝えること:

- Spring Bootのファイルは役割ごとに分かれている
- `controller/`、`service/`、`repository/` のように層ごとに置く
- DTOは `dto/movie/`、`dto/restaurant/` のように題材ごとに分ける
- まず読むのはController、Service、Repository

生成依頼文:

```text
モダンなSaaS技術資料風に、Spring Bootプロジェクトのディレクトリ構造図を作ってください。

左側に backend/src/main/java/com/example/gourmet のツリーを表示してください。
次のように、役割ごとにディレクトリを分けて表示してください。

controller/
  MovieController.java
  RestaurantController.java
service/
  MovieService.java
  RestaurantService.java
repository/
  MovieRepository.java
  RestaurantRepository.java
model/
  Movie.java
  Restaurant.java
  RestaurantStatus.java
dto/
  movie/
    MovieRequest.java
    MovieResponse.java
  restaurant/
    RestaurantRequest.java
    RestaurantResponse.java
右側に、各ファイルの役割をカードで表示してください。
controller/: APIの入口
service/: 処理を書く場所
repository/: SQLを書いてDBとやり取りする場所
model/: DBテーブルの1行を受け取るデータ
dto/: APIで受け渡しするデータ

「まず読む順番」として、controller -> service -> repository を強調してください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、グリーンとシアンを使った読みやすい図にしてください。
```

### 6. Spring Boot基本構造図

ファイル名:

```text
images/spring-basic-flow.png
```

作成済み。

伝えること:

- ControllerはAPIの入口
- Serviceは処理を書く場所
- RepositoryはDBとやり取りする場所
- DBはデータを保存する場所

### 7. Spring Bootクラス名対応図

ファイル名:

```text
images/spring-class-map.png
```

伝えること:

- Controllerという概念は`RestaurantController`に対応する
- Serviceという概念は`RestaurantService`に対応する
- Repositoryという概念は`RestaurantRepository`に対応する

### 8. ClassとDIの関係図

ファイル名:

```text
images/spring-class-di.png
```

伝えること:

- ControllerもServiceもJavaのclassである
- ControllerはServiceを使う
- Controllerが `new MovieService()` するのではなく、Spring BootがServiceを渡す
- `@RestController` と `@Service` はSpring Bootに管理してもらうための目印
- コンストラクタで必要な部品を受け取る

生成依頼文:

```text
モダンなSaaS技術資料風に、Spring BootのClassとDIの関係図を作ってください。

左側に「MovieController class」、右側に「MovieService class」をカードで配置してください。
中央上に「Spring Boot Container」という大きめの枠を置き、Spring BootがMovieControllerとMovieServiceを管理しているように表現してください。

MovieControllerカードには次の要素を入れてください。
@RestController
private final MovieService movieService
public MovieController(MovieService movieService)

MovieServiceカードには次の要素を入れてください。
@Service
public class MovieService

MovieControllerからMovieServiceへ「使いたい」という矢印を出してください。
Spring Boot ContainerからMovieControllerへ「MovieServiceを渡す」という矢印を出してください。

下部に短いまとめとして、
「DI = 必要な部品をSpring Bootに渡してもらう仕組み」
「ControllerはServiceをnewしない」
を表示してください。

白または薄いグレー背景、角丸カード、細い枠線、控えめな影、ネイビー・グリーン・シアンを使って、難しい言葉が怖く見えないように読みやすくしてください。
```

### 9. Controllerの役割図

ファイル名:

```text
images/controller-role.png
```

### 10. Serviceの役割図

ファイル名:

```text
images/service-role.png
```

### 11. Repositoryの役割図

ファイル名:

```text
images/repository-role.png
```

使う場面:

Day3の「Repositoryとは」。

伝えること:

- RepositoryはServiceから呼ばれる
- RepositoryはDB操作の入口である
- `@Select`、`@Insert`、`@Update`、`@Delete` でSQLを書く
- RepositoryにSQLを書くと、DB操作の中身が見える

生成依頼文:

```text
モダンなSaaS技術資料風に、Spring Boot + MyBatisのRepositoryの役割を説明する図を作ってください。

横長16:9の教材スライド画像にしてください。
白または薄いグレー背景、角丸カード、細い枠線、控えめな影、ネイビー・グリーン・シアンを使って、読みやすい図にしてください。

左から右に、次の3つの大きなカードを配置してください。

左: MovieService
中央: MovieRepository
右: Database

MovieServiceからMovieRepositoryへ矢印を出し、ラベルに「DB操作を依頼」と書いてください。
MovieRepositoryからDatabaseへ矢印を出し、ラベルに「SQLを実行」と書いてください。

MovieRepositoryの周辺に、基本操作として次の4つを小さなラベルで表示してください。

@Select
@Insert
@Update
@Delete

MovieRepositoryのカードには、
「DB操作の入口」
「SQLを書く場所」
「MyBatisの@Mapperを付ける」
という説明を入れてください。

Databaseのカードには、
「movies table」
「id / title / genre / memo / imageUrl / status」
という簡単なテーブル例を入れてください。

下部にポイントとして、
「Serviceは何をしたいかを決める」
「RepositoryはSQLを書いてDBとやり取りする」
「Repositoryで扱うのはDTOではなくDBモデル」
を入れてください。

重要:
DTO、Controller、Reactはこの図には入れないでください。
Repositoryの説明だけに集中してください。
文字は日本語中心で、用語は MovieService / MovieRepository / Database / DBモデル / @Mapper / @Select / @Insert / @Update / @Delete を正確に表示してください。
```

## Day 3: DBとCRUD

### 12. CRUDとHTTPメソッド対応図

ファイル名:

```text
images/crud-api-map.png
```

### 13. DBモデルとDBテーブル対応図

ファイル名:

```text
images/entity-table-map.png
```

関連する作成済み画像:

```text
images/spring-entity-flow.png
```

まず `spring-entity-flow.png` は必要に応じてDBモデルの位置づけとして扱い、その後でDBモデルとDBテーブルの対応を説明する。

### 14. 登録APIの流れ

ファイル名:

```text
images/create-api-flow.png
```

### 15. 一覧取得APIの流れ

ファイル名:

```text
images/list-api-flow.png
```

### 16. クエリパラメータの図

ファイル名:

```text
images/query-param-flow.png
```

## Day 4: Reactの基本

### 17. Reactディレクトリ構造図

ファイル名:

```text
images/react-directory-structure.png
```

伝えること:

- Reactのファイルは画面部品ごとに分ける
- `components/` に画面部品を置く
- `api/` にAPI呼び出しをまとめる
- まず読むのはApp、List、Card

### 18. HTML / CSS / JavaScript / React / Tailwind の関係図

ファイル名:

```text
images/react-html-css-js-tailwind.png
```

伝えること:

- HTMLは画面の構造
- CSSは見た目
- JavaScriptは動き
- Reactは構造・見た目・動きをコンポーネントとしてまとめる
- Tailwind CSSは見た目を`className`に書く

### 18. JSXとTailwindの読み方

ファイル名:

```text
images/jsx-tailwind-reading.png
```

伝えること:

- JSXはHTMLに近いがJavaScriptの中に書く
- `className` が見た目
- `{restaurant.name}` がJavaScriptの値
- `onClick` が動き

### 19. Reactコンポーネント分割図

ファイル名:

```text
images/react-components.png
```

### 20. propsの流れ

ファイル名:

```text
images/react-props-flow.png
```

### 21. stateの流れ

ファイル名:

```text
images/react-state-flow.png
```

### 22. フォーム入力とstate

ファイル名:

```text
images/form-state-flow.png
```

### 23. 画像URLとimgタグ

ファイル名:

```text
images/react-image-tag.png
```

## Day 5: API連携とデバッグ

### 24. useEffectでAPIを呼ぶ流れ

ファイル名:

```text
images/useeffect-api-flow.png
```

### 25. fetchの中身

ファイル名:

```text
images/fetch-request-map.png
```

### 26. CORSの図

ファイル名:

```text
images/cors-basic.png
```

### 27. エラー切り分けマップ

ファイル名:

```text
images/debugging-map.png
```

### 28. 開発の進め方

ファイル名:

```text
images/development-flow.png
```
