# Week4 Day1（2026-05-05 / 5月5日（火））
テーマ: 成果物アプリのデータ操作・SQL応用を体験する

## この日の位置づけ
- Week3 までは `SELECT / WHERE / ORDER BY / LIMIT / JOIN` を段階的に読んできた
- 今日はその続きとして、成果物アプリの中で実際にありそうなデータ操作に近い形で SQL を読む
- 「SQL の文法を読む」から「アプリの操作と結びつけて読む」へ進む日

## 到達目標
- 自分のアプリの要件を見て、どんな SQL が必要そうか想像できる
- 複数テーブル JOIN を使った取得 SQL を読んで、何をしたい処理か説明できる
- 公開済みデータの抽出、ユーザー別データ取得、カテゴリ別絞り込みを読める
- 管理画面向けの一覧と公開画面向けの一覧の違いを説明できる
- `WHERE` の条件が増えても、役割ごとに分けて読める
- 「この SQL はどの画面・どの操作のためのものか」を日本語で言える

## 学習時間
- 20:00〜20:45（45分）

## 進め方

### 20:00〜20:05
導入
- `SELECT` = 何を表示するか
- `FROM` = どのテーブルを土台にするか
- `JOIN` = 別テーブルをつなぐ
- `WHERE` = 条件で絞る
- `ORDER BY` = 並び替える
- `LIMIT` = 件数を制限する
- 前回までは要件を SQL に言い換える練習をした
- 今日は、その SQL が実際のアプリの中でどう使われるかを見る

### 20:05〜20:12
まず「どんな操作があるか」を整理する
公開側でありそうな操作:
- 公開済み記事一覧を出す
- 新着記事を出す
- 特定カテゴリの記事一覧を出す
- 記事詳細を表示する
- FAQ 一覧を出す

管理側でありそうな操作:
- 下書きも含めた記事一覧を出す
- 自分が作成した記事だけを見る
- 公開予定日つきの記事を確認する
- カテゴリ別に記事を確認する

ポイント:
- 同じ `articles` テーブルを使っても、公開画面と管理画面では条件が変わる

### 20:12〜20:20
公開画面向けの記事一覧を読む

```sql
SELECT articles.title, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC
LIMIT 10;
```

見るポイント:
- 表示したいもの: 記事タイトル、投稿者名、カテゴリ名、公開日時
- 用途: 公開サイトの新着記事一覧、トップページの最新記事欄
- `WHERE articles.status = 'published'` = 公開済みだけを出す
- `ORDER BY articles.published_at DESC` = 新しい順
- `LIMIT 10` = 最新 10 件

### 20:20〜20:28
管理画面向けの記事一覧を読む

```sql
SELECT articles.title, users.name, categories.name, articles.status, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
ORDER BY articles.created_at DESC;
```

見るポイント:
- `WHERE articles.status = 'published'` がない
- `status` を表示している
- `created_at` で並べている
- 用途: 管理画面の記事一覧、下書きも含めた確認画面

### 20:28〜20:34
ユーザー別の記事取得を読む

```sql
SELECT articles.title, articles.status, articles.published_at
FROM articles
WHERE articles.user_id = 3
ORDER BY articles.created_at DESC;
```

見るポイント:
- `user_id = 3` のユーザーが作った記事だけを見る
- 用途: 自分の記事一覧、担当者別の記事一覧
- 表示したい列が `articles` の中だけなら JOIN は不要

### 20:34〜20:39
カテゴリ別 + 公開済み絞り込みを読む

```sql
SELECT articles.title, categories.name, articles.published_at
FROM articles
JOIN categories ON articles.category_id = categories.id
WHERE categories.name = 'お知らせ'
  AND articles.status = 'published'
ORDER BY articles.published_at DESC;
```

見るポイント:
- カテゴリ名が「お知らせ」
- なおかつ公開済み
- `WHERE` には条件を複数並べられる
- 用途: お知らせカテゴリの公開記事一覧

### 20:39〜20:42
記事詳細 + 投稿者名 + カテゴリ名を読む

```sql
SELECT articles.title, articles.body, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.id = 8;
```

見るポイント:
- `articles.id = 8` の 1 件だけを取る
- 本文・投稿者名・カテゴリ名・公開日時をまとめて表示する
- 用途: 記事詳細画面、管理画面の確認ページ

### 20:42〜20:45
まとめ
- 公開画面向け SQL と管理画面向け SQL の違い
- JOIN が必要な場合と不要な場合の違い
- 条件が 2 つあっても、役割ごとに分ければ読めることを確認する

## この日に使うサンプル SQL

### 1. 公開済み記事の新着 10 件

```sql
SELECT articles.title, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC
LIMIT 10;
```

### 2. 管理画面向けの記事一覧

```sql
SELECT articles.title, users.name, categories.name, articles.status, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
ORDER BY articles.created_at DESC;
```

### 3. 特定ユーザーの記事一覧

```sql
SELECT articles.title, articles.status, articles.published_at
FROM articles
WHERE articles.user_id = 3
ORDER BY articles.created_at DESC;
```

### 4. お知らせカテゴリの公開済み記事一覧

```sql
SELECT articles.title, categories.name, articles.published_at
FROM articles
JOIN categories ON articles.category_id = categories.id
WHERE categories.name = 'お知らせ'
  AND articles.status = 'published'
ORDER BY articles.published_at DESC;
```

### 5. 記事詳細

```sql
SELECT articles.title, articles.body, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.id = 8;
```

## この日のメモに残すとよい内容
- 公開画面向け SQL では `status = 'published'` が入りやすい
- 公開画面では `published_at DESC` で新しい順が多い
- 管理画面では下書きも含めることがある
- 管理画面では `status` を表示したいことがある
- JOIN が必要なのは、投稿者名やカテゴリ名など別テーブルの値を表示したいとき
- `articles` の列だけで足りるなら JOIN は不要
- `WHERE` の条件は複数にできる
- カテゴリ条件 + 公開状態条件、のように組み合わせられる

## 理解チェック
- 公開画面向けの記事一覧で `WHERE articles.status = 'published'` が入りやすいのはなぜか
- 管理画面向け一覧では、なぜ `status` を表示したくなるか
- `SELECT articles.title, articles.status FROM articles ...` のように、JOIN が不要なことがあるのはなぜか
- `WHERE categories.name = 'お知らせ' AND articles.status = 'published'` は何をしているか
- 次の SQL はどんな画面のためのものっぽいか

```sql
SELECT articles.title, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC
LIMIT 10;
```

## 終了条件
- SQL を見て、公開画面向けか管理画面向けかをある程度言える
- JOIN が必要な理由を説明できる
- JOIN が不要な場面もあると分かる
- 条件が 2 つある WHERE を読める
- 自分のアプリでありそうな SQL を、画面イメージつきで説明できる