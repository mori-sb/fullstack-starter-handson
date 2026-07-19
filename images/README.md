# Images

教材で使う概念図やスクリーンショットを置くディレクトリです。

## 作成予定の図

- `app-overview.png`: ブラウザ、React、Spring Boot、DBの全体構成
- `gourmet-app-complete.png`: グルメ管理アプリの完成イメージ
- `app-screen-mock.png`: グルメ管理アプリの完成画面モック
- `app-screen-mock.svg`: グルメ管理アプリの画面完成イメージ
- `http-json-flow.png`: HTTPリクエスト、HTTPレスポンス、JSONの流れ
- `screen-api-map.png`: 画面操作とAPIの対応
- `spring-directory-structure.png`: Spring Bootのディレクトリ構造
- `spring-basic-flow.png`: Browser / React、Controller、Service、Repository、DBの基本の流れ
- `spring-di-container.png`: Spring Boot ContainerとDIの関係
- `spring-class-map.png`: Spring Bootの役割名とクラス名の対応
- `controller-role.png`: Controllerの役割
- `service-role.png`: Serviceの役割
- `repository-role.png`: Repositoryの役割
- `spring-dto-flow.png`: DTOがReactとControllerの間で受け渡される流れ
- `spring-entity-flow.png`: EntityがService、Repository、DBとつながる流れ
- `spring-mapper-flow.png`: DTO、Mapper、Entityの変換
- `dto-entity-mapper.png`: DTO、Entity、Mapperの違い
- `crud-api-map.png`: CRUDとHTTPメソッドの対応
- `dbmodel-table-map.png`: MyBatisで使うDBモデルとDBテーブルの対応
- `dto-dbmodel-flow.png`: Request DTO、DBモデル、Response DTOの使い分け
- `create-api-mybatis-flow.png`: 登録APIでRequest DTOからDB保存、Response DTOまで進む流れ
- `list-api-flow.png`: 一覧取得APIでDBからReact表示まで戻る流れ
- `query-param-flow.png`: クエリパラメータで一覧を絞り込む流れ
- `react-directory-structure.png`: Reactのディレクトリ構造
- `react-html-css-js-tailwind.png`: HTML、CSS、JavaScript、React、Tailwind CSSの関係
- `jsx-tailwind-reading.png`: JSXとTailwind CSSの読み方
- `react-components.png`: Reactコンポーネントの分割
- `react-props-flow.png`: propsが親から子へ渡る流れ
- `react-state-flow.png`: state変更から画面更新までの流れ
- `form-state-flow.png`: フォーム入力とstateの関係
- `react-state-effect.png`: useStateとuseEffectの役割
- `react-image-tag.png`: 画像URLをimgタグで表示する流れ
- `useeffect-api-flow.png`: useEffectでAPIを呼び一覧表示する流れ
- `fetch-request-map.png`: fetchコードとHTTPリクエストの対応
- `image-url-flow.png`: 画像URLがDB、API、Reactを通って表示される流れ
- `cors-basic.png`: CORSの基本
- `development-flow.png`: フルスタック開発の進め方
- `debugging-map.png`: エラー切り分けの確認順序

画像は教材の理解補助として使います。見た目を飾るためだけではなく、コードを読む前に構造をつかむ目的で配置します。

## 作成済み

- `app-screen-mock.svg`: グルメ管理アプリの画面完成イメージ
- `gourmet-app-complete.png`: グルメ管理アプリの完成イメージ
- `spring-basic-flow.png`: Spring Bootバックエンドの基本構造
- `spring-di-container.png`: Spring Boot ContainerとDIの関係
- `spring-dto-flow.png`: Spring BootにおけるDTOの位置づけ
- `spring-entity-flow.png`: Spring BootにおけるEntityの位置づけ
- `spring-mapper-flow.png`: DTO、Mapper、Entityの関係
- `screen-api-map.png`: グルメ管理アプリの画面操作とAPIの対応
- `controller-role.png`: Controllerの役割
- `service-role.png`: Serviceの役割
- `repository-role.png`: Repositoryの役割
- `entity-table-map.png`: EntityとDBテーブルの対応
- `crud-api-map.png`: Spring Boot + MyBatisで作るCRUD APIの対応
- `dbmodel-table-map.png`: MyBatisで使うDBモデルとDBテーブルの対応
- `dto-dbmodel-flow.png`: Request DTO、DBモデル、Response DTOの使い分け
- `create-api-mybatis-flow.png`: Spring Boot + MyBatisの登録APIの流れ
- `query-param-flow.png`: クエリパラメータで一覧を絞り込む流れ
- `react-directory-structure.png`: Reactのディレクトリ構造
- `react-html-css-js-tailwind.png`: HTML、CSS、JavaScript、React、Tailwind CSSの関係
- `jsx-tailwind-reading.png`: JSXとTailwind CSSの読み方
- `react-props-flow.png`: propsが親から子へ渡る流れ
- `form-state-flow.png`: フォーム入力とstateの関係
- `react-image-tag.png`: 画像URLをimgタグで表示する流れ
- `useeffect-api-flow.png`: useEffectでAPIを呼び一覧表示する流れ
- `fetch-request-map.png`: fetchコードとHTTPリクエストの対応
- `cors-basic.png`: CORSの基本

## Spring Boot画像の使う順番

Spring Bootの説明では、次の順番で見せます。

```text
1. spring-basic-flow.png
   Controller -> Service -> Repository -> Database の本線を理解する

2. spring-di-container.png
   ControllerがServiceをnewせず、Spring Bootから受け取るDIを理解する

3. spring-dto-flow.png
   ReactとControllerの間で受け渡すデータの形としてDTOを理解する

4. spring-entity-flow.png
   DBに保存するデータの形としてEntityを理解する

5. spring-mapper-flow.png
   DTOとEntityを変換する役割としてMapperを理解する
```

最初から4枚をまとめて説明しない。
まず基本構造を理解してから、DTO、Entity、Mapperを順番に追加します。
