# Week3 Day1（2026-04-22 / 4月22日（水））
テーマ: PRIMARY KEY / FOREIGN KEY を CREATE TABLE の中で読む

## この日の位置づけ
- Week2 Day2 では `id` / `user_id` / `category_id` の役割を意味で読んだ
- 今日はその続きとして、主キー・外部キーを SQL 上でどう明示するかを見る
- 「列の意味」から「制約としての書き方」へ進む日

## 到達目標
- `PRIMARY KEY` を見て「この列を主キーとして扱う指定」と言える
- `FOREIGN KEY` を見て「別テーブルへの参照を明示する指定」と言える
- `REFERENCES users(id)` を見て「users テーブルの id を参照している」と言える
- `articles.user_id → users.id`
- `articles.category_id → categories.id`
- `faqs.user_id → users.id`
  を SQL の形でも説明できる
- 「外部キーの列」と「外部キー制約」は厳密には別だが、最初はセットで考えてよいと理解できる

## 学習時間
- 20:00〜20:45（45分）

## 進め方

### 20:00〜20:05
導入
- `id` = そのテーブル自身の 1 件を区別する列
- `user_id` = 別テーブルの行を指す列
- `category_id` = 別テーブルの行を指す列
- 前回は列の意味を見た
- 今日は制約としてどう書くかを見る

### 20:05〜20:15
PRIMARY KEY を読む

```sql
CREATE TABLE users (
  id integer NOT NULL,
  name varchar(255) NOT NULL,
  email varchar(255) NOT NULL,
  PRIMARY KEY (id)
);
```

見るポイント:
- `PRIMARY KEY (id)` = このテーブルでは `id` を主キーとして使う
- `id` が users テーブルの 1 件を区別する中心になる
- `id` という列があることと、それを主キーとして扱うことを分けて考える

### 20:15〜20:25
FOREIGN KEY を読む

```sql
CREATE TABLE articles (
  id integer NOT NULL,
  user_id integer NOT NULL,
  category_id integer NOT NULL,
  title varchar(255) NOT NULL,
  body text,
  PRIMARY KEY (id),
  FOREIGN KEY (user_id) REFERENCES users(id),
  FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

見るポイント:
- `FOREIGN KEY (user_id) REFERENCES users(id)` = `user_id` は `users.id` を参照する
- `FOREIGN KEY (category_id) REFERENCES categories(id)` = `category_id` は `categories.id` を参照する
- `user_id` や `category_id` は、ただの integer ではなく他テーブルとつながるための整数

### 20:25〜20:32
列と制約は別だと知る
- `user_id integer NOT NULL` = `user_id` という整数の列がある
- `FOREIGN KEY (user_id) REFERENCES users(id)` = その列が `users.id` を参照するというルール
- 厳密には列と制約は別
- ただし最初は「外部キーの列」としてまとめて理解してよい

### 20:32〜20:38
`faqs` テーブルでも同じ見方をする

```sql
CREATE TABLE faqs (
  id integer NOT NULL,
  user_id integer NOT NULL,
  question text NOT NULL,
  answer text NOT NULL,
  PRIMARY KEY (id),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

見るポイント:
- `faqs.id` は FAQ 1 件を区別する
- `faqs.user_id` は `users.id` を参照する
- FAQ は user とつながっている

### 20:38〜20:42
自分のアプリに当てはめるミニ演習
対象:
- users
- articles
- categories
- faqs

整理すること:

主キー:
- `users.id`
- `articles.id`
- `categories.id`
- `faqs.id`

外部キー候補:
- `articles.user_id` → `users.id`
- `articles.category_id` → `categories.id`
- `faqs.user_id` → `users.id`

### 20:42〜20:45
まとめ
- `PRIMARY KEY (id)` は何を意味するか
- `FOREIGN KEY (user_id) REFERENCES users(id)` は何を意味するか
- `articles.category_id` はどこを参照するか確認する

## この日に使うサンプル SQL

```sql
CREATE TABLE users (
  id integer NOT NULL,
  name varchar(255) NOT NULL,
  email varchar(255) NOT NULL,
  PRIMARY KEY (id)
);
```

```sql
CREATE TABLE articles (
  id integer NOT NULL,
  user_id integer NOT NULL,
  category_id integer NOT NULL,
  title varchar(255) NOT NULL,
  body text,
  PRIMARY KEY (id),
  FOREIGN KEY (user_id) REFERENCES users(id),
  FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

```sql
CREATE TABLE faqs (
  id integer NOT NULL,
  user_id integer NOT NULL,
  question text NOT NULL,
  answer text NOT NULL,
  PRIMARY KEY (id),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

## メモ
- `PRIMARY KEY` = そのテーブルの 1 件を区別するための指定
- `FOREIGN KEY` = 別テーブルを参照するための指定
- `REFERENCES` = どのテーブルのどの列を参照しているかを書く
- `articles.user_id` → `users.id`
- `articles.category_id` → `categories.id`
- `faqs.user_id` → `users.id`
- `user_id` は列
- `FOREIGN KEY (user_id) REFERENCES users(id)` は制約
- ただし最初はセットで理解してよい

## 理解チェック
- `PRIMARY KEY (id)` は何を意味するか
- `FOREIGN KEY (user_id) REFERENCES users(id)` は何を意味するか
- `articles.category_id` はどこを参照するか
- `user_id integer NOT NULL` と `FOREIGN KEY (user_id) REFERENCES users(id)` の違いは何か
- 外部キー制約が付きそうな列を 2 つ以上言えるか

## 終了条件
- `PRIMARY KEY` を見て主キー指定だと分かる
- `FOREIGN KEY ... REFERENCES ...` を見て参照関係を説明できる
- `articles` と `faqs` の参照先を言える
- 列と制約が厳密には別だと薄く理解できる

# Week3 Day2（2026-04-28 / 4月28日（火））
テーマ: JOIN を使って参照関係を読む

## この日の位置づけ
- Week3 Day1 では `PRIMARY KEY` / `FOREIGN KEY` / `REFERENCES` を読んだ
- 今日はその続きとして、参照関係を使って複数テーブルの情報を一緒に読む
- 「つながりがある」と理解した状態から、「そのつながりを使って取得する」へ進む日

## 到達目標
- `JOIN` を見て「別テーブルの情報を一緒に見るためのもの」と言える
- `articles.user_id = users.id` を見て、何と何をつないでいるか説明できる
- `articles.category_id = categories.id` を見て、何と何をつないでいるか説明できる
- `articles` 単体では分からない投稿者名やカテゴリ名を、`JOIN` で一緒に見ていると理解できる
- `SELECT` / `FROM` / `JOIN` / `ON` / `WHERE` を役割ごとにざっくり分けて読める
- 簡単な `JOIN` 文を 2〜3 本読んで、日本語で意味を説明できる

## 学習時間
- 20:00〜20:45（45分）

## 進め方

### 20:00〜20:05
導入
- `articles.user_id` は `users.id` を参照する
- `articles.category_id` は `categories.id` を参照する
- 外部キーがあることで、テーブル同士をつなげられる
- 前回は「つながりがある」と読んだ
- 今日は「そのつながりを使って一緒に見る」

### 20:05〜20:12
JOIN が必要な理由を理解する
- `articles` だけでは、投稿者の番号やカテゴリの番号までは分かっても、名前までは分からない
- 投稿者名を見るには `users`
- カテゴリ名を見るには `categories`
- 別テーブルの情報をくっつけて見るために `JOIN` を使う

### 20:12〜20:22
`articles` と `users` を JOIN する最小例を読む

```sql
SELECT articles.title, users.name
FROM articles
JOIN users ON articles.user_id = users.id;
```

見るポイント:
- `SELECT articles.title, users.name` = 記事タイトルとユーザー名を見たい
- `FROM articles` = articles を土台にする
- `JOIN users` = users をつなぐ
- `ON articles.user_id = users.id` = `articles.user_id` と `users.id` が一致するものを結びつける

この SQL の意味:
- 記事タイトルと、その記事を書いたユーザー名を一緒に見る

### 20:22〜20:30
`articles` と `categories` を JOIN する例を読む

```sql
SELECT articles.title, categories.name
FROM articles
JOIN categories ON articles.category_id = categories.id;
```

見るポイント:
- `articles.category_id` と `categories.id` をつなぐ
- 記事タイトルとカテゴリ名を一緒に見る
- 外部キーが違えば、JOIN の相手も条件も変わる

### 20:30〜20:36
2つ JOIN する形を読む

```sql
SELECT articles.title, users.name, categories.name
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id;
```

見るポイント:
- 土台は `articles`
- `users` をつなぐ
- `categories` もつなぐ
- 記事タイトル・投稿者名・カテゴリ名を同時に見られる
- `JOIN` は 1 回だけとは限らない

### 20:36〜20:40
WHERE を足したときの読み方を知る

```sql
SELECT articles.title, users.name
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published';
```

役割の整理:
- `SELECT` = 何を表示するか
- `FROM` = どこを土台にするか
- `JOIN` = どのテーブルをつなぐか
- `ON` = どういう条件でつなぐか
- `WHERE` = どの行だけに絞るか

この SQL の意味:
- 公開済みの記事だけに絞って、そのタイトルと投稿者名を見る

### 20:40〜20:43
自分のアプリに当てはめるミニ演習
- 「記事タイトルと投稿者名を見たい」→ `articles` と `users` を JOIN
- 「記事タイトルとカテゴリ名を見たい」→ `articles` と `categories` を JOIN
- 「記事タイトル、投稿者名、カテゴリ名を見たい」→ 両方 JOIN

### 20:43〜20:45
まとめ
- `JOIN` は何のために使うか
- `articles.user_id = users.id` は何を意味するか
- `JOIN` と `WHERE` の役割の違いは何か確認する

## この日に使うサンプル SQL

### 1. 記事タイトルと投稿者名を見る

```sql
SELECT articles.title, users.name
FROM articles
JOIN users ON articles.user_id = users.id;
```

### 2. 記事タイトルとカテゴリ名を見る

```sql
SELECT articles.title, categories.name
FROM articles
JOIN categories ON articles.category_id = categories.id;
```

### 3. 記事タイトル・投稿者名・カテゴリ名をまとめて見る

```sql
SELECT articles.title, users.name, categories.name
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id;
```

### 4. 公開済み記事だけに絞る

```sql
SELECT articles.title, users.name
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published';
```

## メモ
- `JOIN` = 別テーブルの情報を一緒に見るためのもの
- `ON` = どの列同士を対応させてつなぐかを書く
- `articles.user_id = users.id`
- `articles.category_id = categories.id`
- `JOIN` はテーブルをつなぐ
- `WHERE` は行を絞る
- 記事タイトルだけなら `articles` 単体でも見られる
- 投稿者名やカテゴリ名まで見たいなら `JOIN` が必要

## 理解チェック
- `JOIN` は何のために使うか
- `articles.user_id = users.id` は何と何をつないでいるか
- `articles.category_id = categories.id` は何と何をつないでいるか
- `SELECT articles.title, users.name` では何を表示したいか
- `JOIN` と `WHERE` の役割の違いは何か

## 終了条件
- `JOIN` を見て「別テーブルの情報を一緒に見る」と説明できる
- `articles.user_id = users.id` の意味を言える
- `articles.category_id = categories.id` の意味を言える
- 2つの簡単な JOIN 文を日本語で説明できる
- `JOIN` と `WHERE` の役割の違いを言える