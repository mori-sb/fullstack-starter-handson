# Day 2: Spring Bootの基本

## 今日のゴール

- Spring Bootプロジェクトの構成を理解する
- Controllerの役割を理解する
- Serviceの役割を理解する
- Repositoryの役割を理解する
- お店一覧APIを作ってJSONで返せる
- 生成されたSpring Bootコードを層ごとに読める

## 扱う内容

- Spring Bootのプロジェクト構成
- Controller / Service / Repository
- DTO
- APIの動作確認
- 画像URLを含むレスポンス

## ライブコーディングと演習

```text
ライブコーディング  MovieController / MovieService
演習              RestaurantController / RestaurantService
```

Movieで作った一覧APIと同じ構造で、Restaurantの一覧APIを作ります。
Day2の演習では、説明していない詳細APIやDB保存は扱いません。

## 進める順番

```text
1. Day1の全体像を復習し、今日はバックエンドだけを見る
2. Spring Bootのディレクトリ構造とファイルの役割を確認する
3. Controller、Service、Repositoryの流れを図で確認する
4. DTOとJSONレスポンスの形を確認する
5. 固定データを返す一覧APIを段階的に実装する
6. ブラウザまたはAPIクライアントで動作確認する
7. 演習の進め方と確認ポイントを確認する
```

Day2では、DB保存までは急ぎません。
まずは「APIの入口にリクエストが届き、JSONが返る」ことを理解します。

## 今日の大事な考え方

Spring Bootでは、処理を役割ごとに分けて書く。

```text
Controller  APIの入口
Service     業務処理を書く場所
Repository  DBアクセスを書く場所
Entity      DBに保存するデータの形
DTO         APIで受け渡しするデータの形
```

AIがコードを生成した場合も、まず「このコードはどの役割か」を見る。

## 最初に伝えること

Spring Bootのコードは、1つのファイルに全部書かない。

APIの入口、処理、DBアクセスを分けて書く。
この分け方を理解すると、生成されたコードを読めるようになる。

最初に覚える本線はこれ。

```text
Controller -> Service -> Repository -> DB
```

DTO、Entity、Mapperは後で追加する概念。
最初から全部覚えようとせず、まずはこの一本道を理解する。

![Spring Bootバックエンドの基本構造](../images/spring-basic-flow.png)

この図では、まずSpring Bootの本線だけを見ます。

```text
Browser / React -> Controller -> Service -> Repository -> Database
```

DTO、Entity、Mapperはまだ覚えなくて大丈夫です。
最初は「APIの入口」「処理を書く場所」「DBとやり取りする場所」の3つに分けて考えます。

## Spring Bootのディレクトリ構造

Spring Boot側は `backend/` に作ります。

まずは、どのファイルが何の役割なのかを見えるようにします。

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
      │     └─ mapper/
      │        ├─ MovieMapper.java
      │        └─ RestaurantMapper.java
      └─ resources/
         └─ application.yml
```

この教材では、Controller、Service、Repositoryなどの役割がすぐ見つかるように、層ごとにディレクトリを分けます。
`dto/` はMovie用とRestaurant用で分け、APIで受け渡しするデータの形を探しやすくします。

最初に見るファイル:

```text
controller/RestaurantController.java
service/RestaurantService.java
repository/RestaurantRepository.java
```

あとから見るファイル:

```text
entity/Restaurant.java
dto/restaurant/RestaurantRequest.java
dto/restaurant/RestaurantResponse.java
mapper/RestaurantMapper.java
entity/RestaurantStatus.java
```

ファイル名を見るだけでも、おおよその役割が分かるようにしておきます。

```text
controller/   APIの入口
service/      処理を書く場所
repository/   DBとやり取りする場所
dto/          ReactとAPIで受け渡しするデータ
mapper/       DTOとEntityを変換する
entity/       DBに保存するデータ
```

## 役割を日常の言葉で考える

### Controller

受付。

外から来たリクエストを受け取る。
URLとHTTPメソッドを見て、どの処理を呼ぶか決める。

![Controllerの役割](../images/controller-role.png)

```text
GET /api/restaurants が来た
  -> RestaurantServiceに一覧取得をお願いする
```

### Service

担当者。

アプリとして何をするかを書く場所。
登録する、一覧を取得する、編集する、削除するなどの処理の中心。

![Serviceの役割](../images/service-role.png)

### Repository

DB係。

DBに保存する、DBから取得する、DBから削除するなどを担当する。
ServiceはRepositoryを通してDBにアクセスする。

![Repositoryの役割](../images/repository-role.png)

### DTO

APIで受け渡しするデータの形。

Reactから送られてくるJSON、Reactへ返すJSONをJavaの形で表す。

Day2では「APIの入出力の形」くらいの理解でよい。

![Spring BootにおけるDTOの位置づけ](../images/spring-dto-flow.png)

DTOはReactとSpring Bootが安全にデータを受け渡すための形です。

```text
Request DTO   Reactから送られるJSONの形
Response DTO  Reactへ返すJSONの形
```

Day2では、DTOを「APIで使うデータの形」として理解できれば十分です。

## コードを読む順番

Spring Bootのコードを見るときは、次の順番で読むと迷いにくい。

```text
1. Controllerを見る
   どのURLとHTTPメソッドを受け取るか確認する

2. Serviceを見る
   実際に何をしているか確認する

3. Repositoryを見る
   DBとどうやり取りしているか確認する

4. DTOを見る
   APIでどんなJSONを受け渡しするか確認する
```

AIにコードを生成してもらった後も、この順番で読む。
動いたかどうかだけでなく、どの層に何が書かれているかを見る。

## 変更したい内容と見るファイル

```text
APIのURLを確認したい
  -> controller/RestaurantController.java

登録や一覧取得の処理を確認したい
  -> service/RestaurantService.java

DBへの保存・取得を確認したい
  -> repository/RestaurantRepository.java

APIで受け取るJSONの形を確認したい
  -> dto/restaurant/RestaurantRequest.java

APIで返すJSONの形を確認したい
  -> dto/restaurant/RestaurantResponse.java

DBに保存する項目を確認したい
  -> entity/Restaurant.java
```

## 図で確認すること

- Controller / Service / Repository の役割図
- リクエストがControllerに届いてレスポンスが返るまでの流れ
- お店データがJSONとして返る流れ

## API処理の流れ

```text
GET /api/restaurants
  ↓
RestaurantController
  ↓
RestaurantService
  ↓
RestaurantRepository
  ↓
DB
  ↓
JSONでレスポンス
```

## ハンズオン

固定のお店データを返す一覧APIを作成する。

```text
GET /api/restaurants
```

返すJSONの例:

```json
[
  {
    "id": 1,
    "name": "Cafe Sakura",
    "area": "新宿",
    "genre": "カフェ",
    "memo": "落ち着いて作業できそう",
    "imageUrl": "https://example.com/cafe.jpg",
    "status": "WANT_TO_GO"
  }
]
```

## 実装の進め方

一気に全部作らず、次の順番で作ります。

### 1. Controllerだけで固定文字列を返す

まずはAPIにアクセスできるか確認します。

```java
@GetMapping
public String hello() {
    return "restaurants api";
}
```

1行ずつ読む:

```text
@GetMapping
  GETリクエストを受け取るメソッドだとSpring Bootに伝える。

public String hello()
  APIが呼ばれたときに実行されるメソッド。
  Stringを返すので、文字列のレスポンスになる。

return "restaurants api";
  ブラウザやAPIクライアントへ返す文字列。
```

確認すること:

```text
GET /api/restaurants にアクセスできる
404ではない
Spring Bootが起動している
```

### 2. DTOを作ってJSONを返す

次に、Reactへ返したい形を `RestaurantResponse` として作ります。

```java
public record RestaurantResponse(
        Long id,
        String name,
        String area,
        String genre,
        String memo,
        String imageUrl,
        String status
) {
}
```

1行ずつ読む:

```text
public record RestaurantResponse(...)
  Reactへ返すデータの形を定義している。
  recordは、値をまとめて持つためのJavaの書き方。

Long id
  お店を区別するID。

String name
  店名。

String area
  地域。

String genre
  ジャンル。

String memo
  メモ。

String imageUrl
  画像URL。画像ファイル本体ではなく、URL文字列を持つ。

String status
  行きたい、行った、お気に入りなどの状態。
```

見るポイント:

- フィールド名がJSONのキーになる
- `imageUrl` も普通の文字列として扱う
- ReactはこのJSONを受け取って画面に表示する

### 3. ControllerからDTOのリストを返す

```java
@GetMapping
public List<RestaurantResponse> findAll() {
    return List.of(
            new RestaurantResponse(
                    1L,
                    "Cafe Sakura",
                    "新宿",
                    "カフェ",
                    "落ち着いて作業できそう",
                    "https://example.com/cafe.jpg",
                    "WANT_TO_GO"
            )
    );
}
```

1行ずつ読む:

```text
@GetMapping
  GET /api/restaurants が来たときに、このメソッドを動かす。

public List<RestaurantResponse> findAll()
  RestaurantResponseを複数件返すメソッド。
  Listなので、JSONでは配列として返る。

return List.of(...)
  固定のお店データをリストとして返す。

new RestaurantResponse(...)
  Reactへ返す1件分のお店データを作っている。

1L
  idの値。Long型なのでLを付けている。

"Cafe Sakura"
  nameに入る値。

"新宿"
  areaに入る値。

"カフェ"
  genreに入る値。

"落ち着いて作業できそう"
  memoに入る値。

"https://example.com/cafe.jpg"
  imageUrlに入る値。

"WANT_TO_GO"
  statusに入る値。
```

確認すること:

```text
レスポンスがJSON配列になっている
name, area, genre, memo, imageUrl, status が含まれている
画像URLが文字列として返っている
```

### 4. Serviceへ処理を移す

Controllerに直接データを書くと、APIの入口と処理が混ざります。
そのため、一覧を作る処理をServiceへ移します。

```text
Controller  リクエストを受け取る
Service     返すデータを用意する
```

この分け方を早めに覚えておくと、後で登録、編集、削除を追加しやすくなります。

## 演習

固定データを3件に増やします。

条件:

- 地域が異なるお店を入れる
- ジャンルが異なるお店を入れる
- `imageUrl` を全件に入れる
- `status` を `WANT_TO_GO`、`VISITED`、`FAVORITE` のいずれかにする

確認すること:

- APIを呼ぶと3件のJSONが返る
- 各データに必要な項目が入っている
- ControllerとServiceの役割を説明できる

## AIへの依頼例

Day2では、まず固定データを返す一覧APIを作ります。
AIには、役割を分けることと、作るファイルを明確に伝えます。

```text
Spring Bootでグルメ管理アプリのお店一覧APIを作ってください。

作るAPI:
GET /api/restaurants

返す項目:
id, name, area, genre, memo, imageUrl, status

作るファイル:
controller/RestaurantController.java
service/RestaurantService.java
dto/restaurant/RestaurantResponse.java

条件:
- ControllerはAPIの入口だけを担当してください
- 固定データはServiceで作ってください
- DB接続はまだ使わないでください
- レスポンスはJSON配列にしてください

作成後に、ControllerとServiceの役割の違いを説明してください。
```

AIの回答を確認するときのポイント:

- Controllerに処理を書きすぎていないか
- Serviceが一覧データを返しているか
- `RestaurantResponse` に必要な項目が揃っているか
- `imageUrl` が文字列として含まれているか
- `GET /api/restaurants` で呼べる形になっているか

## コードサンプル

最初はDBを使わず、固定データを返して流れを理解する。

```java
@RestController
@RequestMapping("/api/restaurants")
public class RestaurantController {

    @GetMapping
    public List<RestaurantResponse> findAll() {
        return List.of(
                new RestaurantResponse(
                        1L,
                        "Cafe Sakura",
                        "新宿",
                        "カフェ",
                        "落ち着いて作業できそう",
                        "https://example.com/cafe.jpg",
                        "WANT_TO_GO"
                )
        );
    }
}
```

この時点では、Controllerだけでも動かせる。
ただし実務では処理が増えるので、ServiceやRepositoryに分けていく。

## 動作確認の観点

ブラウザやAPIクライアントで次のURLにアクセスする。

```text
http://localhost:8080/api/restaurants
```

確認すること:

- JSONが返ってくる
- `name`、`area`、`genre`、`memo`、`imageUrl`、`status` が含まれている
- 画像URLはただの文字列として返っている
- Reactで表示する前に、API単体で確認できる

## 今日の確認ポイント

- Controllerの役割を説明できる
- Serviceの役割を説明できる
- Repositoryの役割を説明できる
- APIがJSONを返すことを確認できる
- 固定データでも、Reactとつなぐ前のAPI確認として意味があると理解できる

## よくある混乱

### Controllerに全部書いてはいけないのですか

小さいサンプルなら動く。

ただし処理が増えると読みにくくなる。
そのため、Controllerは受付に集中し、処理はServiceに分ける。

### Serviceは必ず必要ですか

実務では入れることが多い。

理由は、APIの入口と処理の中身を分けた方が変更しやすいから。

### RepositoryはSQLを書く場所ですか

必ずしも手でSQLを書く場所ではない。

Spring Data JPAを使うと、基本的なCRUDはRepositoryのメソッドで扱える。

## メモ

Springの用語が多くなるので、最初は役割を日常的な言葉に置き換えて説明する。
