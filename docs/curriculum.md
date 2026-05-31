# Curriculum

この教材は、Spring BootとReactの基本構造を理解し、自分で小さな実装を進められる状態を目指します。

コードを丸写しすることを目的にしません。
講義で見た構造を、別の題材に置き換えて実装することで、理解して書ける状態を作ります。

## 全体方針

進め方は、すべての日で次の形に統一します。

```text
説明
  ↓
Movie題材でライブコーディング
  ↓
Restaurant題材で同じ構造を演習
```

ライブコーディングでは `Movie` を使います。
演習では `Restaurant` を使います。

参加者は、Movieの実装を見ながら、クラス名、ファイル名、項目名、APIのURLをRestaurantへ置き換えて実装します。
資料ではMovie側の英語名を例として見せ、Restaurant側の名前は演習で埋めます。

```text
controller/MovieController.java
service/MovieService.java
repository/MovieRepository.java
entity/Movie.java
dto/movie/MovieRequest.java
dto/movie/MovieResponse.java
MovieCard
MovieList
MovieForm
/api/movies
```

## 演習の出し方

演習では、説明していない内容を出しません。

悪い例:

```text
ライブコーディング:
GET /api/movies の一覧取得だけ説明する

演習:
GET /api/restaurants/{id} の詳細取得を作らせる
```

これは避けます。
参加者は、説明されていない `PathVariable`、ID検索、404処理を急に扱うことになり、構造をなぞる練習になりません。

良い例:

```text
ライブコーディング:
GET /api/movies の一覧取得を説明する

演習:
GET /api/restaurants の一覧取得を同じ構造で作る
```

ライブコーディングと演習は、必ず同じ難易度、同じ構造にします。

## 小さく作って肉付けする方針

この教材では、最初から完成形を目指しません。
小さいところから作り、動くことを確認してから、少しずつ機能を足します。

基本の流れ:

```text
1. まず最小のコードを書く
2. 動くか確認する
3. コードを読む
4. 役割を分ける
5. 項目や機能を足す
6. もう一度動作確認する
```

Spring Bootの例:

```text
1. Controllerで文字列を返す
2. DTOでJSONを返す
3. Serviceへ処理を移す
4. RepositoryでDBにつなぐ
5. 登録、編集、削除を追加する
```

Reactの例:

```text
1. App.jsxに固定データを書く
2. Cardで1件表示する
3. Listで複数件表示する
4. Formで入力できるようにする
5. stateを更新して画面を変える
6. API連携へ進む
```

コピペ用コードを使う場合も、完成コードを貼って終わりにはしません。
貼った後に、どの行が何をしているか、どのファイルが何を担当しているかを確認します。

## コード解説の方針

ライブコーディングでは、コピペしてよいです。
ただし、貼る単位は小さくし、次の単位で止めながら説明します。

```text
1. このファイルは何の役割か
2. このメソッドは誰から呼ばれるか
3. 引数には何が入るか
4. 戻り値はどこへ返るか
5. 次にどのファイルへ処理が進むか
```

コードサンプルには、重要な行ごとに解説を入れます。

例:

```java
@GetMapping
public List<MovieResponse> findAll() {
    return movieService.findAll();
}
```

読み方:

```text
@GetMapping
  GET /api/movies を受け取る。

public List<MovieResponse> findAll()
  MovieResponseを複数件返すメソッド。
  JSONでは配列として返る。

return movieService.findAll();
  実際の処理はServiceへ任せる。
```

## Day 1: Webアプリの全体像

### 今日のゴール

- Webアプリが、画面、API、DBに分かれていることを理解する
- React、Spring Boot、DBの役割を説明できる
- HTTPリクエスト、HTTPレスポンス、JSONの流れを理解する
- 画面操作とAPIの対応を考えられる
- BrunoでAPIを呼ぶとJSONが返ることを確認できる

### 使用する図

- `app-overview.png`: ブラウザ、React、Spring Boot、DBの全体構成
- `http-json-flow.png`: HTTPリクエスト、レスポンス、JSONの流れ
- `screen-api-map.png`: 画面操作とAPIの対応
- `app-screen-mock.svg`: グルメ管理アプリの画面完成イメージ

### 講義で説明する内容

- ブラウザ、React、Spring Boot、DBの役割
- ReactはDBを直接触らないこと
- Spring Boot APIがJSONを返すこと
- JSONはデータの受け渡し形式であり、DBではないこと
- REST APIでは、URLとHTTPメソッドで操作を表すこと
- 画面操作とAPIが対応すること

### ライブコーディング内容

Day1では本格的なコード実装はしません。
代わりに、Movie題材でAPIの設計を見せます。

Movieの画面操作とAPI:

```text
映画一覧を見る      GET    /api/movies
映画を登録する      POST   /api/movies
映画を編集する      PUT    /api/movies/{id}
映画を削除する      DELETE /api/movies/{id}
```

MovieのJSON例:

```json
{
  "id": 1,
  "title": "Inception",
  "genre": "SF",
  "memo": "夢の中に入っていく映画",
  "imageUrl": "https://example.com/inception.jpg",
  "status": "WATCHED"
}
```

Brunoで見せる内容:

```http
GET http://localhost:8080/api/movies
```

返るJSON:

```json
[
  {
    "id": 1,
    "title": "Inception",
    "genre": "SF",
    "memo": "夢の中に入っていく映画",
    "imageUrl": "https://example.com/inception.jpg",
    "status": "WATCHED"
  }
]
```

### 演習内容

Restaurant題材で、画面操作とAPIの対応を作ります。

演習で作るもの:

```text
お店一覧を見る      GET    /api/restaurants
お店を登録する      POST   /api/restaurants
お店を編集する      PUT    /api/restaurants/{id}
お店を削除する      DELETE /api/restaurants/{id}
地域で絞り込む      GET    /api/restaurants?area=新宿
```

RestaurantのJSON例も作ります。

```json
{
  "id": 1,
  "name": "Cafe Sakura",
  "area": "新宿",
  "genre": "カフェ",
  "memo": "落ち着いて作業できそう",
  "imageUrl": "https://example.com/cafe.jpg",
  "status": "WANT_TO_GO"
}
```

### ライブコーディングと演習の対応表

| ライブコーディング | 演習 |
| --- | --- |
| Movieの画面操作を整理する | Restaurantの画面操作を整理する |
| `/api/movies` を考える | `/api/restaurants` を考える |
| MovieのJSONを見る | RestaurantのJSONを書く |
| BrunoでMovie APIを見る | BrunoでRestaurant APIを見る準備をする |

### 完成コードのゴール

Day1では完成コードは作りません。
次の日から作るコードの地図を作ることがゴールです。

### 動作確認方法

- Brunoを開く
- `GET http://localhost:8080/api/movies` を送る
- JSON配列が返ることを見る
- JSONのキーと画面項目が対応していることを確認する

### よくあるエラー

- Spring Bootが起動していない
- URLが間違っている
- ポート番号が違う
- JSONではなくエラーページが返っている

### 参加者が理解すべきポイント

- ReactはAPIを呼ぶ
- APIはJSONを返す
- ReactはJSONを画面に表示する
- APIのURLは画面操作と対応する

### チェックリスト

- React、Spring Boot、DBの役割を説明できる
- HTTPリクエストとレスポンスを説明できる
- JSONがどこで使われるか説明できる
- BrunoでAPIを呼ぶ流れを説明できる
- Restaurantの画面操作とAPI対応を書ける

## Day 2: Spring Bootの基本

### 今日のゴール

- ControllerとServiceの役割を理解する
- GET APIを作ってJSONを返せる
- Movieの実装をRestaurantへ置き換えられる
- 生成されたSpring Bootコードを役割ごとに読める

### 使用する図

- `spring-basic-flow.png`: Browser / React、Controller、Service、Repository、DBの基本の流れ
- `controller-role.png`: Controllerの役割
- `service-role.png`: Serviceの役割
- `spring-dto-flow.png`: DTOの位置づけ

### 講義で説明する内容

- ControllerはAPIの入口
- Serviceは処理を書く場所
- DTOはAPIで返すデータの形
- Controllerに処理を書きすぎず、Serviceへ任せること
- `GET /api/movies` がJSON配列を返す流れ

### ライブコーディング内容

Movie題材で一覧APIを作ります。

作るファイル:

```text
controller/MovieController.java
service/MovieService.java
dto/movie/MovieResponse.java
```

API:

```text
GET /api/movies
```

MovieResponse:

```java
public record MovieResponse(
        Long id,
        String title,
        String genre,
        String memo,
        String imageUrl,
        String status
) {
}
```

Controller:

```java
@RestController
@RequestMapping("/api/movies")
public class MovieController {

    private final MovieService movieService;

    public MovieController(MovieService movieService) {
        this.movieService = movieService;
    }

    @GetMapping
    public List<MovieResponse> findAll() {
        return movieService.findAll();
    }
}
```

1行ずつ読む:

```text
@RestController
  このクラスがAPIのControllerであることを表す。

@RequestMapping("/api/movies")
  このControllerのURLの基本部分を指定する。

private final MovieService movieService;
  ControllerからServiceを呼ぶためのフィールド。

public MovieController(MovieService movieService)
  SpringがMovieServiceを渡してくれる。

@GetMapping
  GET /api/movies を受け取る。

public List<MovieResponse> findAll()
  MovieResponseを複数件返す。

return movieService.findAll();
  実際の一覧取得はServiceへ任せる。
```

Service:

```java
@Service
public class MovieService {

    public List<MovieResponse> findAll() {
        return List.of(
                new MovieResponse(
                        1L,
                        "Inception",
                        "SF",
                        "夢の中に入っていく映画",
                        "https://example.com/inception.jpg",
                        "WATCHED"
                )
        );
    }
}
```

### 演習内容

Restaurant題材で同じ構造を作ります。

作るファイル:

```text
controller/RestaurantController.java
service/RestaurantService.java
dto/restaurant/RestaurantResponse.java
```

API:

```text
GET /api/restaurants
```

演習では、Movieの構造をRestaurantへ置き換えます。
資料上ではMovie側の例だけを見せ、Restaurant側は参加者が考えて埋めます。

```text
Controller      MovieController
Service         MovieService
Response DTO    MovieResponse
API             GET /api/movies
JSON keys       id, title, genre, memo, imageUrl, status
```

### ライブコーディングと演習の対応表

| Movie側で見たもの | Restaurant側で考えること |
| --- | --- |
| `MovieController` | Controller名 |
| `MovieService` | Service名 |
| `MovieResponse` | Response DTO名 |
| `GET /api/movies` | 一覧APIのURL |
| `title` | 店名にあたるJSONキー |

### 完成コードのゴール

- Brunoで `GET /api/restaurants` を呼べる
- JSON配列が返る
- ControllerがServiceを呼んでいる
- Serviceが固定データを返している

### 動作確認方法

```http
GET http://localhost:8080/api/restaurants
```

確認すること:

- ステータスコードが成功
- JSON配列が返る
- `id`、`name`、`area`、`genre`、`memo`、`imageUrl`、`status` が含まれる

### よくあるエラー

- `@RestController` を付け忘れてAPIとして認識されない
- `@RequestMapping` のURLが間違っている
- Serviceを呼ばずにControllerに全部書いている
- `List` のimportがない
- Brunoで古いURLを叩いている

### 参加者が理解すべきポイント

- Controllerはリクエストを受け取る
- Serviceは返すデータを用意する
- DTOのフィールド名がJSONのキーになる
- Movieの構造をRestaurantへ置き換えられる

### チェックリスト

- `RestaurantController` を作った
- `RestaurantService` を作った
- `RestaurantResponse` を作った
- `GET /api/restaurants` がJSONを返す
- ControllerとServiceの役割を説明できる

## Day 3: DB接続とCRUD API

### 今日のゴール

- Entity、Repository、DTO、Mapperの役割を理解する
- DB保存を使ったCRUD APIを作れる
- Movie CRUDの構造をRestaurant CRUDへ置き換えられる
- 説明したAPIだけを演習で実装できる

### 使用する図

- `spring-entity-flow.png`: Entityの位置づけ
- `entity-table-map.png`: EntityとDBテーブルの対応
- `spring-mapper-flow.png`: DTO、Mapper、Entityの関係
- `repository-role.png`: Repositoryの役割

### 講義で説明する内容

- EntityはDBに保存するデータの形
- RepositoryはDB操作の入口
- Request DTOはReactから受け取る形
- Response DTOはReactへ返す形
- MapperはDTOとEntityを変換する
- CRUDはCreate、Read、Update、Deleteの基本操作

### ライブコーディング内容

Movie題材でCRUD APIを作ります。
演習でRestaurant CRUDを作るため、ライブコーディングで扱ったAPIだけを演習対象にします。

作るファイル:

```text
entity/Movie.java
repository/MovieRepository.java
dto/movie/MovieRequest.java
dto/movie/MovieResponse.java
mapper/MovieMapper.java
service/MovieService.java
controller/MovieController.java
```

API:

```text
GET    /api/movies
GET    /api/movies/{id}
POST   /api/movies
PUT    /api/movies/{id}
DELETE /api/movies/{id}
```

Entity:

```java
@Entity
public class Movie {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String title;
    private String genre;
    private String memo;
    private String imageUrl;
    private String status;
}
```

1行ずつ読む:

```text
@Entity
  DBに保存するクラスであることを表す。

@Id
  主キーを表す。

@GeneratedValue(strategy = GenerationType.IDENTITY)
  idをDBに自動採番してもらう。

private String title;
  DBに保存する映画タイトル。
```

Repository:

```java
public interface MovieRepository extends JpaRepository<Movie, Long> {
}
```

Controllerの登録API:

```java
@PostMapping
public MovieResponse create(@RequestBody MovieRequest request) {
    return movieService.create(request);
}
```

読み方:

```text
@PostMapping
  POST /api/movies を受け取る。

@RequestBody MovieRequest request
  リクエストJSONをMovieRequestとして受け取る。

return movieService.create(request);
  登録処理はServiceへ任せる。
```

### 演習内容

Restaurant題材で同じCRUD APIを作ります。

作るファイル:

```text
entity/Restaurant.java
repository/RestaurantRepository.java
dto/restaurant/RestaurantRequest.java
dto/restaurant/RestaurantResponse.java
mapper/RestaurantMapper.java
service/RestaurantService.java
controller/RestaurantController.java
```

API:

```text
GET    /api/restaurants
GET    /api/restaurants/{id}
POST   /api/restaurants
PUT    /api/restaurants/{id}
DELETE /api/restaurants/{id}
```

Movieで説明していない検索条件や複雑なバリデーションは出しません。

### ライブコーディングと演習の対応表

| Movie側で見たもの | Restaurant側で考えること |
| --- | --- |
| `Movie` | Entity名 |
| `MovieRepository` | Repository名 |
| `MovieRequest` | Request DTO名 |
| `MovieResponse` | Response DTO名 |
| `MovieMapper` | Mapper名 |
| `/api/movies/{id}` | ID付きAPIのURL |
| `title` | 店名にあたるJSONキー |

### 完成コードのゴール

- RestaurantをDBに保存できる
- 一覧、詳細、登録、更新、削除APIが動く
- DTOとEntityを分けている
- Mapperで変換している
- ServiceからRepositoryを呼んでいる

### 動作確認方法

Brunoで確認します。

```http
POST http://localhost:8080/api/restaurants
GET  http://localhost:8080/api/restaurants
GET  http://localhost:8080/api/restaurants/1
PUT  http://localhost:8080/api/restaurants/1
DELETE http://localhost:8080/api/restaurants/1
```

### よくあるエラー

- Entityに `@Id` がない
- Repositoryの型指定が違う
- Request DTOとEntityを混同している
- Mapperで `id` の扱いを間違える
- 存在しないIDを指定したときの扱いがない

### 参加者が理解すべきポイント

- EntityはDB用
- DTOはAPI用
- Mapperは変換用
- RepositoryはDB操作用
- ControllerはServiceを呼ぶ

### チェックリスト

- `Restaurant` Entityを作った
- `RestaurantRepository` を作った
- Request/Response DTOを作った
- Mapperを作った
- CRUD APIをBrunoで確認した
- MovieとRestaurantの対応を説明できる

## Day 4: Reactの基本

### 今日のゴール

- JSX、props、stateを理解する
- コンポーネントを分割できる
- フォーム入力をstateで管理できる
- Movie画面の構造をRestaurant画面へ置き換えられる

### 使用する図

- `react-html-css-js-tailwind.png`: HTML、CSS、JavaScript、React、Tailwind CSSの関係
- `jsx-tailwind-reading.png`: JSXとTailwind CSSの読み方
- `react-directory-structure.png`: Reactのディレクトリ構造
- `react-props-flow.png`: propsの流れ
- `form-state-flow.png`: フォーム入力とstateの関係

### 講義で説明する内容

- JSXはHTMLに近い構造を書くもの
- `className` にTailwind CSSを書く
- propsは親から子へ渡すデータ
- stateは画面の中で変わるデータ
- フォーム入力はstateで管理する
- コンポーネントは役割ごとに分ける

### ライブコーディング内容

Movie題材でReact画面を作ります。

作るファイル:

```text
MovieCard.jsx
MovieList.jsx
MovieForm.jsx
```

MovieCard:

```jsx
export function MovieCard({ movie }) {
  return (
    <article className="rounded-lg border bg-white p-4 shadow-sm">
      <img
        className="h-40 w-full rounded object-cover"
        src={movie.imageUrl}
        alt={movie.title}
      />
      <h3 className="mt-3 text-lg font-semibold">{movie.title}</h3>
      <p className="text-sm text-slate-600">{movie.genre}</p>
      <p className="mt-2 text-sm">{movie.memo}</p>
      <span className="mt-3 inline-block rounded bg-slate-100 px-2 py-1 text-xs">
        {movie.status}
      </span>
    </article>
  );
}
```

1行ずつ読む:

```text
export function MovieCard({ movie })
  movieをpropsとして受け取る。

<article className="...">
  カード全体の箱。

src={movie.imageUrl}
  画像URLをimgタグへ渡す。

alt={movie.title}
  画像の代替テキスト。

{movie.title}
  JavaScriptの値を画面に表示する。
```

MovieList:

```jsx
export function MovieList({ movies }) {
  return (
    <div className="grid gap-4">
      {movies.map((movie) => (
        <MovieCard key={movie.id} movie={movie} />
      ))}
    </div>
  );
}
```

### 演習内容

Restaurant題材で同じ構造を作ります。

作るファイル:

```text
RestaurantCard.jsx
RestaurantList.jsx
RestaurantForm.jsx
```

参考にするMovie側の名前:

```text
MovieCard
MovieList
MovieForm
movie
movies
title
```

### ライブコーディングと演習の対応表

| Movie側で見たもの | Restaurant側で考えること |
| --- | --- |
| `MovieCard` | カードコンポーネント名 |
| `MovieList` | 一覧コンポーネント名 |
| `MovieForm` | フォームコンポーネント名 |
| `movie` | 1件分のprops名 |
| `movies` | 配列state名 |
| `title` | 店名にあたる表示キー |

### 完成コードのゴール

- 固定データでRestaurant一覧を表示できる
- RestaurantCardが1件分を表示する
- RestaurantListが配列を受け取って一覧表示する
- RestaurantFormが入力値をstateで管理する

### 動作確認方法

- Reactを起動する
- 画面にRestaurantカードが表示される
- 画像、店名、地域、ジャンル、メモ、ステータスが表示される
- フォームに入力できる
- Consoleにエラーがない

### よくあるエラー

- props名が一致していない
- `map` の `key` がない
- `restaurant.imageUrl` のURLが空
- `useState` のimportがない
- `className` ではなく `class` と書いている

### 参加者が理解すべきポイント

- propsは親から子へ渡る
- stateが変わると画面が更新される
- `map` で配列をコンポーネント一覧に変換する
- JSX、className、JavaScriptの値が1つのコンポーネントにまとまる

### チェックリスト

- `RestaurantCard` を作った
- `RestaurantList` を作った
- `RestaurantForm` を作った
- propsの流れを説明できる
- stateの役割を説明できる

## Day 5: ReactとAPI連携

### 今日のゴール

- ReactからSpring Boot APIを呼べる
- `fetch` と `useEffect` の役割を理解する
- JSONレスポンスをstateに入れて画面表示できる
- Movie API連携をRestaurant API連携へ置き換えられる

### 使用する図

- `fetch-request-map.png`: fetchコードとHTTPリクエストの対応
- `useeffect-api-flow.png`: useEffectでAPIを呼ぶ流れ
- `cors-basic.png`: CORSの基本
- `react-image-tag.png`: 画像URLをimgタグで表示する流れ

### 講義で説明する内容

- `fetch` はAPIを呼ぶために使う
- `useEffect` は画面表示時の処理に使う
- APIレスポンスをstateに入れると画面が更新される
- CORSはブラウザの安全機能
- API呼び出し関数は `api/` にまとめる

### ライブコーディング内容

Movie題材でAPI連携を作ります。

作るファイル:

```text
api/movies.js
App.jsx
MovieList.jsx
MovieForm.jsx
```

API関数:

```jsx
const API_BASE_URL = "http://localhost:8080/api/movies";

export async function fetchMovies() {
  const response = await fetch(API_BASE_URL);
  return response.json();
}

export async function createMovie(movie) {
  const response = await fetch(API_BASE_URL, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(movie),
  });

  return response.json();
}
```

1行ずつ読む:

```text
const API_BASE_URL
  APIのURLをまとめる。

fetchMovies()
  一覧取得APIを呼ぶ関数。

await fetch(API_BASE_URL)
  GET /api/movies を呼ぶ。

response.json()
  JSONレスポンスをJavaScriptのデータに変換する。

createMovie(movie)
  登録APIを呼ぶ関数。

method: "POST"
  新規登録を表すHTTPメソッド。

JSON.stringify(movie)
  JavaScriptのオブジェクトをJSON文字列に変換する。
```

useEffect:

```jsx
useEffect(() => {
  async function loadMovies() {
    const data = await fetchMovies();
    setMovies(data);
  }

  loadMovies();
}, []);
```

### 演習内容

Restaurant題材で同じAPI連携を作ります。

作るファイル:

```text
api/restaurants.js
App.jsx
RestaurantList.jsx
RestaurantForm.jsx
```

API:

```text
GET  /api/restaurants
POST /api/restaurants
```

Day5の演習では、一覧取得と登録に絞ります。
編集、削除、複雑なフィルタは、説明してから別の演習にします。

### ライブコーディングと演習の対応表

| Movie側で見たもの | Restaurant側で考えること |
| --- | --- |
| `api/movies.js` | API呼び出しファイル名 |
| `fetchMovies` | 一覧取得関数名 |
| `createMovie` | 登録関数名 |
| `movies` state | 一覧state名 |
| `setMovies` | 一覧stateの更新関数名 |
| `/api/movies` | APIのURL |

### 完成コードのゴール

- React起動時にRestaurant一覧APIを呼ぶ
- JSONレスポンスをstateに入れる
- RestaurantListに表示される
- フォーム送信でRestaurant登録APIを呼ぶ
- 登録後に一覧が更新される

### 動作確認方法

- Spring Bootを起動する
- Reactを起動する
- Brunoで `GET /api/restaurants` を確認する
- React画面で一覧が表示される
- フォームから登録する
- BrunoまたはReact画面で登録結果を確認する
- NetworkでAPIリクエストを見る

### よくあるエラー

- CORSエラー
- API URLが間違っている
- Spring Bootが起動していない
- `response.json()` を忘れている
- `setRestaurants(data)` を呼んでいない
- `Content-Type` を付けずにPOSTしている

### 参加者が理解すべきポイント

- ReactはAPIを呼ぶ
- APIレスポンスはJSON
- JSONをstateに入れると画面が変わる
- API呼び出し関数はコンポーネントから分ける
- Movie API連携をRestaurant API連携へ置き換えられる

### チェックリスト

- `api/restaurants.js` を作った
- `fetchRestaurants` を作った
- `createRestaurant` を作った
- `useEffect` で一覧取得している
- API結果を `restaurants` stateに入れている
- フォーム送信で登録APIを呼んでいる
- NetworkでAPI通信を確認できる

## Gitリポジトリ構成案

```text
fullstack-starter-handson/
├─ README.md
├─ docs/
│  ├─ README.md
│  ├─ curriculum.md
│  ├─ getting-started.md
│  ├─ day1.md
│  ├─ day2.md
│  ├─ day3.md
│  ├─ day4.md
│  ├─ day5.md
│  ├─ glossary.md
│  ├─ checklists.md
│  └─ instructor/
│     ├─ notes.md
│     ├─ design-policy.md
│     ├─ visual-map.md
│     ├─ code-samples.md
│     └─ exercise-answers.md
├─ images/
├─ backend/
│  └─ src/
└─ frontend/
   └─ src/
```

## ブランチ案

Dayごとに、開始状態と完成状態を分けます。

```text
main
develop

day1/start
day1/complete

day2/start
day2/movie-live
day2/restaurant-exercise-complete

day3/start
day3/movie-live
day3/restaurant-exercise-complete

day4/start
day4/movie-live
day4/restaurant-exercise-complete

day5/start
day5/movie-live
day5/restaurant-exercise-complete
```

使い方:

```text
start
  その日の開始状態。

movie-live
  講義で作ったMovie実装。

restaurant-exercise-complete
  演習の模範解答。
```

参加者は `start` から作業し、詰まったときは `movie-live` を見て構造を確認します。
答え合わせやAI採点では `restaurant-exercise-complete` を参照します。
