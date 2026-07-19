# Getting Started

このページは、ハンズオンを始めるときに最初に読むページです。

このハンズオンでは、最初からすべてを暗記したり、完璧に理解したりする必要はありません。

まずは、次の3つを目指します。

```text
1. 画面、API、DBの役割が分かる
2. コードがどの役割のものか見分けられる
3. 小さな変更を自分で試せる
```

## プロジェクトを始める方法

環境準備は、参加前にすべて終わっている前提にしません。
プロジェクトを始めるタイミングで、次の順番で確認します。

```text
1. GitHubからリポジトリを取得する
2. developブランチに移動する
3. backendをIntelliJ IDEAで開く
4. Spring Bootを起動できるか確認する
5. frontendをVS Codeで開く
6. Reactを起動できるか確認する
7. BrunoでAPI確認の準備をする
8. Day3でDBを使うタイミングでRancher Desktopを準備する
```

最初から全部を完璧に整えるより、使うタイミングで確認します。
Day1とDay2では、まずWebアプリ全体像とSpring Boot APIの基本を理解します。
DBコンテナはDay3で必要になるため、そのタイミングで起動確認します。

### 使うツールをインストールする

最初に、開発に必要なツールを入れます。
すでに入っている場合は、この節は確認だけで大丈夫です。

この教材では、macOSでの作業を想定します。
社内PCの権限やセキュリティ設定によってインストール方法が違う場合は、社内ルールに従います。

入れるもの:

```text
Java 21             Spring Bootを動かす
Maven               Spring Bootプロジェクトをビルドする
Git                 GitHubからリポジトリを取得する
Node.js / yarn      Reactを動かす
IntelliJ IDEA       Spring Boot側を書く
VS Code             React側を書く
Bruno               APIを確認する
Rancher Desktop     DBコンテナを起動する
```

#### Homebrewを使う場合

Macでは、Homebrewを使うとコマンドでツールを入れられます。
Homebrewが入っているか確認します。

```bash
brew --version
```

表示されない場合は、公式サイトの手順に従ってHomebrewをインストールします。

```text
https://brew.sh/
```

Homebrewを入れた直後は、ターミナルに表示される案内に従ってPATH設定を反映します。
PATH設定ができていないと、`brew` コマンドが見つからないことがあります。

#### Java 21をインストールする

Spring BootはJavaで動きます。
この教材ではJava 21を使います。

Homebrewを使う場合:

```bash
brew install --cask temurin@21
```

公式ページから入れる場合:

```text
https://adoptium.net/temurin/releases/?version=21
```

インストール後に確認します。

```bash
java -version
```

見るポイント:

```text
21 が表示される
```

Javaは「入っているか」だけでなく、「IntelliJ IDEAがそのJavaを使っているか」も大事です。
あとでIntelliJ IDEAのProject SDKもJava 21に合わせます。

#### Mavenをインストールする

Mavenは、Spring Bootプロジェクトをビルドするために使います。
`pom.xml` を読み、必要なライブラリを取得して、アプリを起動・ビルドできるようにします。

Homebrewを使う場合:

```bash
brew install maven
```

公式ページ:

```text
https://maven.apache.org/download.cgi
```

インストール後に確認します。

```bash
mvn -v
```

見るポイント:

```text
Apache Maven のバージョンが表示される
Java version が 21 になっている
```

IntelliJ IDEAだけで実行する場合でも、Mavenの考え方は出てきます。
`pom.xml` を変更したら、Mavenを再読み込みする必要があります。

#### Node.js / yarnをインストールする

Node.jsは、Reactを動かすために使います。
yarnは、Reactで使うライブラリを入れたり、開発サーバーを起動したりするために使います。

Homebrewを使う場合:

```bash
brew install node
```

公式ページから入れる場合は、LTS版を選びます。

```text
https://nodejs.org/
```

インストール後に確認します。

```bash
node -v
corepack enable
yarn -v
```

見るポイント:

```text
node のバージョンが表示される
yarn のバージョンが表示される
```

#### IntelliJ IDEAをインストールする

IntelliJ IDEAは、Spring Boot側のJavaコードを書くために使います。
Community Editionでも基本的なJava開発はできます。
Spring Boot支援機能を多く使う場合はUltimate Editionが便利です。

公式ページ:

```text
https://www.jetbrains.com/idea/download/
```

インストール後に確認すること:

```text
IntelliJ IDEAを起動できる
backend/ を開ける
Java 21をProject SDKに設定できる
Mavenプロジェクトとして読み込める
```

#### VS Codeをインストールする

VS Codeは、React側のコードを書くために使います。

公式ページ:

```text
https://code.visualstudio.com/
```

インストール後に確認すること:

```text
VS Codeを起動できる
frontend/ を開ける
ターミナルを開ける
```

Macで `code .` コマンドを使いたい場合は、VS CodeのCommand Paletteから次を実行します。

```text
Shell Command: Install 'code' command in PATH
```

#### Brunoをインストールする

Brunoは、APIを直接呼び出して確認するために使います。
React画面を作る前に、Spring Boot APIが正しくJSONを返すか確認できます。

Homebrewを使う場合:

```bash
brew install bruno
```

公式ページ:

```text
https://www.usebruno.com/downloads
```

インストール後に確認すること:

```text
Brunoを起動できる
WorkspaceまたはCollectionを作成できる
GETリクエストを作成できる
```

#### Rancher Desktopをインストールする

Rancher Desktopは、DBコンテナを起動するために使います。
Day1、Day2ではまだDBを使わないため、実際の起動確認はDay3に入る前で大丈夫です。

公式ページ:

```text
https://rancherdesktop.io/
```

インストール後に確認すること:

```text
Rancher Desktopを起動できる
Container Engineを使える
docker コマンドが使える
```

Day3に入る前に確認します。

```bash
docker --version
docker ps
```

`docker` コマンドが使えない場合は、Rancher Desktopの設定でContainer EngineやPATH設定を確認します。

### 最初の環境確認

ターミナルで次のコマンドを実行します。

```bash
java -version
mvn -v
node -v
yarn -v
git --version
```

確認できればよいこと:

```text
Javaのバージョンが表示される
Mavenのバージョンが表示される
Node.jsのバージョンが表示される
yarnのバージョンが表示される
Gitのバージョンが表示される
```

コマンドが見つからない場合は、実装に入る前にツールのインストールまたはPATH設定を確認します。

### 1. リポジトリを取得する

GitHubにある教材リポジトリを自分のPCに取得します。

```bash
git clone <repository-url>
cd fullstack-starter-handson
```

すでにclone済みの場合は、作業ディレクトリに移動します。

```bash
cd fullstack-starter-handson
```

確認すること:

```bash
pwd
git status
```

見るポイント:

```text
今いる場所が fullstack-starter-handson になっている
git status が表示できる
```

### 2. developブランチに移動する

作業は `develop` ブランチで進めます。

```bash
git branch
git switch develop
```

`develop` がまだ手元にない場合は、次のように取得します。

```bash
git fetch origin
git switch -c develop origin/develop
```

確認すること:

```bash
git branch
```

見るポイント:

```text
* develop
```

`*` が付いているブランチが、今いるブランチです。

### 3. backendをIntelliJ IDEAで開く

Spring Boot側は `backend/` です。
IntelliJ IDEAでは、リポジトリ全体ではなく `backend/` を開くとSpring Bootプロジェクトとして扱いやすいです。

開く場所:

```text
fullstack-starter-handson/backend
```

最初に見るファイル:

```text
backend/pom.xml
backend/src/main/java/com/example/gourmet/GourmetApplication.java
backend/src/main/resources/application.yml
```

Spring Bootプロジェクトを開くときは、どのプロジェクトでもまず次の3つを探します。

```text
pom.xml
  このプロジェクトをどうビルドするかを書くファイル。
  Mavenがこのファイルを読んで、必要なライブラリを取得する。

GourmetApplication.java
  Spring Bootアプリの起動入口。
  このファイルを実行すると、APIサーバーが起動する。

application.yml
  アプリの設定を書くファイル。
  ポート番号、DB接続先、ログ設定などを書く。
```

この3つを見ると、別のSpring Bootプロジェクトでも次のことを判断できます。

```text
どんなライブラリを使っているか
どのJavaファイルからアプリを起動するか
どのポートやDBに接続するか
```

#### pom.xmlを見る理由

`pom.xml` は、Mavenプロジェクトの設定ファイルです。
Spring Bootでは、必要な機能を依存関係として追加します。

例:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

読み方:

```text
spring-boot-starter-web
  Web APIを作るために必要。
  Controller、HTTPリクエスト、JSONレスポンスなどを扱えるようにする。
```

Day3でDBを使うときは、次のような依存関係が必要になります。

```text
spring-boot-starter-data-jpa
  EntityやRepositoryを使ってDB操作をするために必要。

postgresql
  Spring BootからPostgreSQLへ接続するために必要。
```

IntelliJ IDEAで `pom.xml` を開いたら、Mavenの読み込みが完了しているか確認します。
読み込みが終わっていないと、`@RestController` や `@Service` などのSpring Bootのクラスを正しく認識できません。

よく見る状態:

```text
Mavenの読み込み中
  依存関係を取得している途中。
  少し待つ。

赤いエラーが多い
  Maven読み込みが終わっていないか、Java SDKが合っていない可能性がある。

pom.xmlを変更した
  Mavenの再読み込みが必要。
```

IntelliJ IDEAでの操作の目安:

```text
pom.xmlを開く
  -> Mavenとして読み込むか聞かれたら読み込む

Mavenウィンドウを開く
  -> Reload All Maven Projects を押す

依存関係を追加した
  -> もう一度 Reload All Maven Projects を押す
```

Mavenの読み込みが終わると、Spring Bootのアノテーションやimportの赤いエラーが減ります。
それでも赤い場合は、Java SDKの設定を確認します。

#### Java SDKを見る理由

Spring BootはJavaで動きます。
そのため、IntelliJ IDEAがどのJavaを使うかを設定する必要があります。

この教材では、Java 21を使います。

確認すること:

```text
Project SDK が 21 になっている
MavenのJavaバージョンと合っている
GourmetApplication.java を実行できる
```

Java SDKが合っていないと、次のような問題が起きます。

```text
Javaの文法エラーが出る
Mavenビルドが失敗する
Spring Bootアプリを起動できない
```

別プロジェクトでも、最初に `pom.xml` のJavaバージョンとIntelliJ IDEAのProject SDKが合っているか確認します。

IntelliJ IDEAでの操作の目安:

```text
Project Structure を開く
  -> Project SDK を確認する
  -> 21 を選ぶ

Module SDK も確認する
  -> Project SDK と同じJavaを使う
```

`pom.xml` に次のように書かれている場合、IntelliJ側もJava 21に合わせます。

```xml
<java.version>21</java.version>
```

#### GourmetApplication.javaを見る理由

`GourmetApplication.java` は、Spring Bootアプリの起動入口です。

```java
@SpringBootApplication
public class GourmetApplication {

    public static void main(String[] args) {
        SpringApplication.run(GourmetApplication.class, args);
    }
}
```

見るポイント:

```text
@SpringBootApplication
  このclassをSpring Bootアプリの起点にする。

mainメソッド
  Javaアプリを起動するときの入口。

SpringApplication.run(...)
  Spring Bootアプリを起動する。
```

このファイルがある場所も大事です。
ControllerやServiceは、基本的にこのファイルと同じパッケージ配下に置きます。

この教材では、起動クラスがここにあります。

```text
com/example/gourmet/GourmetApplication.java
```

そのため、ControllerやServiceは次のように置きます。

```text
com/example/gourmet/controller/
com/example/gourmet/service/
```

別プロジェクトでも、まず起動クラスを探すと、どこにControllerやServiceを置けばSpring Bootが見つけてくれるか判断しやすくなります。

#### application.ymlを見る理由

`application.yml` は、アプリの設定を書くファイルです。

この教材では、最初はポート番号だけを確認します。

```yaml
server:
  port: 8080
```

読み方:

```text
server.port
  Spring Bootをどのポートで起動するか。
  8080なら http://localhost:8080 でAPIを呼べる。
```

Day3でDBを使うと、DB接続先もここに書きます。

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/gourmet
    username: gourmet
    password: password
```

別プロジェクトでも、APIのURLやDB接続で迷ったら、まず `application.yml` を見ます。

#### 起動確認

IntelliJ IDEAで確認すること:

```text
Mavenの読み込みが完了している
Java SDKが設定されている
GourmetApplication.java を実行できる
```

Spring Bootを起動できたら、ログに次のような内容が出ます。

```text
Tomcat started on port 8080
Started GourmetApplication
```

このログが出たら、Spring BootのAPIサーバーが起動しています。
このあと、Brunoやブラウザから `http://localhost:8080/...` にアクセスしてAPIを確認できます。

### 4. frontendをVS Codeで開く

React側は `frontend/` です。

開く場所:

```text
fullstack-starter-handson/frontend
```

Reactプロジェクトを作成済みの場合は、VS Codeのターミナルで次を実行します。

```bash
yarn install
yarn dev
```

起動できたら、ブラウザで次を開きます。

```text
http://localhost:5173
```

まだReactプロジェクトを作っていない場合は、Day4で作ります。
この時点では、`frontend/` がReact側の作業場所であることを確認できれば大丈夫です。

### 5. BrunoでAPI確認の準備をする

Brunoは、React画面を作る前にAPIを直接確認するために使います。

最初に作るリクエスト:

```text
Name: Get movies
Method: GET
URL: http://localhost:8080/api/movies
```

Day2でMovie APIを作ったあと、このリクエストを送ってJSONが返ることを確認します。

Day3以降は、Restaurant API用のリクエストも追加します。

```text
Name: Get restaurants
Method: GET
URL: http://localhost:8080/api/restaurants
```

### 6. Day3でDBを準備する

DBはDay3で使います。
Day1、Day2の時点では、DBコンテナを起動していなくても進められます。

Day3に入る前に、Rancher Desktopを起動してから確認します。

```bash
docker --version
docker ps
```

リポジトリに `compose.yml` または `docker-compose.yml` がある場合は、次でDBを起動します。

```bash
docker compose up -d
```

DB接続で見る場所:

```text
backend/src/main/resources/application.yml
```

ReactからDBへ直接つなぐことはありません。
ReactはSpring Boot APIを呼び、Spring BootがDBとやり取りします。

## 参加前に知っておくと楽になること

このハンズオンでは、Java、JavaScript、TypeScriptを深く知っている前提にはしません。
ただし、次の言葉を少しだけ見ておくと、当日の説明が追いやすくなります。

完璧に覚える必要はありません。
「聞いたことがある」「コードを見たときに何となく役割が分かる」くらいで十分です。

### Webアプリの最低限

知っておきたいこと:

```text
ブラウザ       ユーザーが操作する画面
フロントエンド Reactで作る画面側
バックエンド   Spring Bootで作るAPI側
API           画面とバックエンドの窓口
HTTPメソッド   GET、POST、PUT、DELETE
JSON          ReactとSpring Bootが受け渡すデータ形式
DB            データを保存する場所
```

まず大事なのは、ReactがDBを直接触らないことです。
ReactはAPIを呼び、Spring BootがDBとやり取りします。

### Javaの最低限

Spring Boot側ではJavaを書きます。

知っておきたいこと:

```text
class       処理やデータのまとまり
method      classの中に書く処理
field       classが持つ値
constructor classを作るときに呼ばれる入口
record      値をまとめて持つための書き方
List        複数件のデータを扱う入れ物
```

このハンズオンで特に大事なのは `class` と `method` です。

例:

```java
public class MovieService {

    public List<MovieResponse> findAll() {
        return List.of();
    }
}
```

読み方:

```text
MovieService
  映画に関する処理を書くclass

findAll()
  一覧取得をするmethod

List<MovieResponse>
  MovieResponseを複数件返す
```

### JavaScript / TypeScriptの最低限

React側では、JavaScriptまたはTypeScriptに近い書き方を使います。
この教材では `.jsx` を中心に扱いますが、TypeScriptの考え方も少し出てきます。

知っておきたいこと:

```text
const        変数を定義する
function     処理をまとめる
object       nameやareaなどのまとまり
array        複数件のデータ
map          配列を1件ずつ画面表示へ変換する
props        親から子コンポーネントへ渡すデータ
state        画面の中で変化するデータ
type         値の形を表す考え方
```

Reactでよく見る形:

```jsx
const restaurants = [
  { id: 1, name: "Cafe Sakura", area: "新宿" },
];

restaurants.map((restaurant) => (
  <RestaurantCard key={restaurant.id} restaurant={restaurant} />
));
```

読み方:

```text
restaurants
  お店の配列

map
  配列を1件ずつ取り出す

restaurant
  取り出した1件分のお店

RestaurantCard
  お店1件を表示するコンポーネント
```

### 事前学習の目安

時間がある場合は、次の順番で軽く見ておきます。

```text
1. Webアプリの全体像
2. JSONとHTTPメソッド
3. Javaのclassとmethod
4. JavaScriptのobject、array、function
5. Reactのcomponent、props、state
6. TypeScriptの基本的な型
```

全部を先に理解しようとしなくて大丈夫です。
ハンズオン中に、必要なところだけ戻って確認します。

### 参考サイト

公式ドキュメントを中心に、必要なところだけ見ます。

- [Java Tutorials: Classes and Objects](https://docs.oracle.com/javase/tutorial/java/javaOO/index.html)
- [Spring Boot: Spring Beans and Dependency Injection](https://docs.spring.io/spring-boot/reference/using/spring-beans-and-dependency-injection.html)
- [Spring Framework: Dependency Injection](https://docs.spring.io/spring-framework/reference/core/beans/dependencies/factory-collaborators.html)
- [MDN: JavaScript Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions)
- [MDN: JavaScript Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
- [TypeScript Handbook: Everyday Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html)
- [React: Passing Props to a Component](https://react.dev/learn/passing-props-to-a-component)
- [React: State: A Component's Memory](https://react.dev/learn/state-a-components-memory)

## 使うツール

このハンズオンでは、次のツールを使います。

```text
IntelliJ IDEA       Spring Bootのコードを書く
VS Code             Reactのコードを書く
Rancher Desktop     DBコンテナを起動する
Bruno               APIの動作確認をする
Git / GitHub        コードと資料を管理する
Java                Spring Bootを動かす
Maven               Spring Bootプロジェクトをビルドする
Node.js / yarn      Reactを動かす
```

各ツールの設定は、上の「プロジェクトを始める方法」で使う順番に確認します。
DBはDay3で使うため、Rancher Desktopの確認もDay3に入る前で大丈夫です。

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

たとえば、次のような使い方を想定します。

```text
気になるカフェを見つけた
  -> 「行きたい」として登録する

ランチがよかった店がある
  -> 「お気に入り」としてメモを残す

作業しやすい店を探したい
  -> 地域やジャンルで絞り込む
```

完成画面では、お店がカード形式で並びます。
カードには画像、店名、地域、ジャンル、ステータス、メモが表示されます。

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
