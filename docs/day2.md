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
      │     └─ restaurant/
      │        ├─ RestaurantController.java
      │        ├─ RestaurantService.java
      │        ├─ RestaurantRepository.java
      │        ├─ Restaurant.java
      │        ├─ RestaurantRequest.java
      │        ├─ RestaurantResponse.java
      │        ├─ RestaurantMapper.java
      │        └─ RestaurantStatus.java
      └─ resources/
         └─ application.yml
```

最初に見るファイル:

```text
RestaurantController.java
RestaurantService.java
RestaurantRepository.java
```

あとから見るファイル:

```text
Restaurant.java
RestaurantRequest.java
RestaurantResponse.java
RestaurantMapper.java
RestaurantStatus.java
```

ファイル名を見るだけでも、おおよその役割が分かるようにしておきます。

```text
Controller  APIの入口
Service     処理を書く場所
Repository  DBとやり取りする場所
Request     Reactから受け取るデータ
Response    Reactへ返すデータ
Mapper      Request/ResponseとEntityを変換する
Entity      DBに保存するデータ
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
  -> RestaurantController.java

登録や一覧取得の処理を確認したい
  -> RestaurantService.java

DBへの保存・取得を確認したい
  -> RestaurantRepository.java

APIで受け取るJSONの形を確認したい
  -> RestaurantRequest.java

APIで返すJSONの形を確認したい
  -> RestaurantResponse.java

DBに保存する項目を確認したい
  -> Restaurant.java
```

## 使用する図

- Controller / Service / Repository の役割図
- リクエストがControllerに届いてレスポンスが返るまでの流れ
- お店データがJSONとして返る流れ

## 図の説明メモ

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
