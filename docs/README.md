# Hands-on Docs

このディレクトリには、ハンズオンで使う資料を置いています。

まずは、次の順番で読んでください。

```text
1. getting-started.md
2. day1.md
3. day2.md
4. day3.md
5. day4.md
6. day5.md
```

分からない言葉が出てきたら `glossary.md`、作業確認をしたいときは `checklists.md` を見ます。

## 1日の進め方

各日は、約3時間の講義と、その後の演習で進めます。

講義では、いきなり完成コードを書くのではなく、次の順番で理解していきます。

```text
1. 今日作るものを確認する
2. 図で全体の流れをつかむ
3. どのファイルに何を書くか確認する
4. 講師が小さく実装して見せる
5. 動作確認する
6. 参加者が演習で同じ考え方を使う
```

コードはAIで生成してもよいですが、生成されたコードをそのまま貼って終わりにはしません。
必ず「どのファイルが、どの役割を持ち、どのデータを受け渡しているか」を確認します。

## 講義と演習の時間配分

目安は次の通りです。

```text
00:00-00:20  今日作るものとゴール確認
00:20-01:00  概念説明と図の確認
01:00-01:40  コードの読み方、ファイル構成の確認
01:40-02:30  ライブ実装
02:30-03:00  動作確認、よくあるエラー、演習説明
03:00-       演習
```

演習では、完成コードを目指すだけでなく、次の説明ができる状態を目指します。

- どの画面操作で、どのAPIが呼ばれるか
- どのファイルに処理が書かれているか
- APIで送るJSONと返るJSONの形
- エラーが出たときに最初に見る場所

## 参加者向け

- [Getting Started](getting-started.md)
- [Day 1: Webアプリの全体像](day1.md)
- [Day 2: Spring Bootの基本](day2.md)
- [Day 3: DB接続とCRUD API](day3.md)
- [Day 4: Reactの基本](day4.md)
- [Day 5: ReactとAPI連携](day5.md)
- [Glossary](glossary.md)
- [Checklists](checklists.md)

## 講師向け

講師・教材作成者向けのメモは `instructor/` にまとめています。

- [Instructor Notes](instructor/notes.md)
- [Design Policy](instructor/design-policy.md)
- [Visual Map](instructor/visual-map.md)
- [Code Samples](instructor/code-samples.md)
