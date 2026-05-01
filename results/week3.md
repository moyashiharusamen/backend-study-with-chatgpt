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

# Week3 Day3（2026-04-30 / 4月30日（木））
テーマ: JOIN + WHERE + ORDER BY をまとめて読む

## この日の位置づけ
- Week3 Day2 では `JOIN` を使って参照関係を読むところまで進んだ
- 今日はその続きとして、`JOIN` した結果に `WHERE` と `ORDER BY` を足して読む
- 一覧画面っぽい SQL を読む最初の日

## 到達目標
- `SELECT` / `FROM` / `JOIN` / `ON` / `WHERE` / `ORDER BY` を役割ごとに分けて読める
- `JOIN` はテーブルをつなぐ、`WHERE` は行を絞る、`ORDER BY` は並び順を決めると説明できる
- `articles` と `users` を JOIN した SQL を読んで、どんな一覧を作りたいかを日本語で言える
- 公開済み記事だけに絞る SQL を読める
- 新しい順、古い順、名前順などの並び替えを読める
- 簡単な一覧取得 SQL を 2〜3 本読んで意味を説明できる

## 学習時間
- 20:00〜20:45（45分）

## 進め方

### 20:00〜20:05
導入
- `JOIN` = 別テーブルの情報を一緒に見る
- `ON` = どの列同士をつなぐかを書く
- `articles.user_id = users.id`
- `articles.category_id = categories.id`
- 前回は「つなぐ」まで
- 今日は「つないだあとに絞って並べる」

### 20:05〜20:12
まず役割を整理する
- `SELECT` = 何を表示するか
- `FROM` = どのテーブルを土台にするか
- `JOIN` = どのテーブルを追加でつなぐか
- `ON` = どの列同士を対応させるか
- `WHERE` = どの行だけに絞るか
- `ORDER BY` = どんな順番で並べるか
- `LIMIT` = 何件まで表示するか

### 20:12〜20:22
JOIN + WHERE を読む

```sql
SELECT articles.title, users.name
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published';
```

見るポイント:
- `SELECT articles.title, users.name` = 記事タイトルと投稿者名を表示したい
- `FROM articles` = articles を土台にする
- `JOIN users ON articles.user_id = users.id` = users をつないで投稿者名を取れるようにする
- `WHERE articles.status = 'published'` = 公開済みの記事だけに絞る

この SQL の意味:
- 公開済みの記事だけを対象にして、記事タイトルと投稿者名を表示する

### 20:22〜20:30
JOIN + ORDER BY を読む

```sql
SELECT articles.title, users.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC;
```

見るポイント:
- `ORDER BY articles.published_at DESC` = 公開日の新しい順に並べる
- `DESC` = 降順 = 新しい順でよく使う
- `ASC` = 昇順 = 古い順や A→Z など

この SQL の意味:
- 公開済みの記事を、新しい公開日順に並べて、タイトル・投稿者名・公開日時を表示する

### 20:30〜20:35
categories も含めた一覧を読む

```sql
SELECT articles.title, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC;
```

見るポイント:
- `users` をつないで投稿者名を取る
- `categories` をつないでカテゴリ名を取る
- 公開済み記事だけに絞る
- 公開日の新しい順に並べる

この SQL の意味:
- 公開済みの記事一覧を、新しい順で、タイトル・投稿者名・カテゴリ名・公開日時つきで表示する

### 20:35〜20:39
LIMIT を足したときの読み方を知る

```sql
SELECT articles.title, users.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC
LIMIT 5;
```

見るポイント:
- `LIMIT 5` = 5件まで表示する

この SQL の意味:
- 公開済みの記事を新しい順に並べて、上から 5 件だけ表示する

### 20:39〜20:42
自分のアプリに当てはめるミニ演習
- 「公開済みの記事のタイトルと投稿者名を見たい」→ `articles` と `users` を JOIN、`WHERE articles.status = 'published'`
- 「公開済みの記事を新しい順で見たい」→ `ORDER BY articles.published_at DESC`
- 「公開済みの記事を 5 件だけ見たい」→ `LIMIT 5`

### 20:42〜20:45
まとめ
- `JOIN` と `WHERE` の違い
- `ORDER BY` は何をするか
- `LIMIT` は何をするか
- この 3 つを言えるか確認する

## この日に使うサンプル SQL

### 1. 公開済み記事のタイトルと投稿者名を見る

```sql
SELECT articles.title, users.name
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published';
```

### 2. 公開済み記事を新しい順に見る

```sql
SELECT articles.title, users.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC;
```

### 3. 投稿者名とカテゴリ名も含めた一覧を見る

```sql
SELECT articles.title, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC;
```

### 4. 上位 5 件だけ表示する

```sql
SELECT articles.title, users.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC
LIMIT 5;
```

## メモ
- `SELECT` = 何を表示するか
- `FROM` = どこを土台にするか
- `JOIN` = どのテーブルをつなぐか
- `ON` = どの列同士をつなぐか
- `WHERE` = どの行だけに絞るか
- `ORDER BY` = どんな順番で並べるか
- `LIMIT` = 何件まで表示するか
- `JOIN` はテーブルをつなぐ
- `WHERE` は行を絞る
- `DESC` = 降順 = 新しい順でよく使う
- 記事タイトルだけなら `articles` 単体でも見られる
- 投稿者名やカテゴリ名まで見たいなら `JOIN` が必要

## 理解チェック
- `WHERE articles.status = 'published'` は何をしているか
- `ORDER BY articles.published_at DESC` は何をしているか
- `LIMIT 5` は何をしているか
- `JOIN` と `WHERE` の役割の違いは何か
- 次の SQL はどんな一覧を作りたい文か

```sql
SELECT articles.title, users.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
WHERE articles.status = 'published'
ORDER BY articles.published_at DESC
LIMIT 5;
```

## 終了条件
- `SELECT` / `FROM` / `JOIN` / `WHERE` / `ORDER BY` / `LIMIT` を役割ごとに読める
- `JOIN` と `WHERE` の違いを言える
- `ORDER BY ... DESC` を見て「新しい順」と読める
- `LIMIT` を見て件数制限だと分かる
- 記事一覧系の簡単な SQL を日本語で説明できる

# Week3 Day4（2026-05-01 / 5月1日（金））
テーマ: 自分のアプリでよくある取得 SQL を読む

## この日の位置づけ
- Week3 Day3 では `JOIN` + `WHERE` + `ORDER BY` + `LIMIT` をまとめて読んだ
- 今日はその続きとして、実務や成果物アプリで本当に出てきそうな一覧・詳細・絞り込み SQL を読む
- 「文法を読む」から「用途を読む」へ進む日

## 到達目標
- SQL を見て「一覧用」「詳細用」「絞り込み用」などの用途を言える
- `articles` が主役の SQL なのか、`faqs` が主役の SQL なのかを見分けられる
- `WHERE id = ...` を見て「1件詳細を取りたい SQL」と読める
- `WHERE status = 'published'` を見て「公開済みだけの一覧」と読める
- `JOIN` がある理由を「表示したい列が別テーブルにあるから」と説明できる
- 自分の成果物アプリでありそうな SQL を 3〜4 本読んで意味を説明できる

## 学習時間
- 20:00〜20:45（45分）

## 進め方

### 20:00〜20:05
導入
- `SELECT` = 何を表示するか
- `FROM` = どこを土台にするか
- `JOIN` = どのテーブルをつなぐか
- `WHERE` = どの行だけに絞るか
- `ORDER BY` = どんな順番で並べるか
- `LIMIT` = 何件まで表示するか
- 前回は SQL の部品を読んだ
- 今日は、その SQL がどんな画面で使われそうかまで考える

### 20:05〜20:12
一覧用 SQL と詳細用 SQL の違いを知る
- 一覧用 SQL = 複数件を取る
- 新着記事一覧、公開済み記事一覧、FAQ 一覧など
- よく出るもの: `WHERE`, `ORDER BY`, `LIMIT`

- 詳細用 SQL = 1件を取る
- 記事詳細、FAQ 詳細など
- よく出るもの: `WHERE id = ...`

### 20:12〜20:20
記事一覧っぽい SQL を読む

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
- 用途: 公開済み記事の一覧画面、新着記事一覧っぽい
- `JOIN` が必要な理由: 投稿者名は `users`、カテゴリ名は `categories` にあるから
- `LIMIT 10` があるので、新着 10 件っぽい

### 20:20〜20:27
記事詳細っぽい SQL を読む

```sql
SELECT articles.title, articles.body, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.id = 5;
```

見るポイント:
- `WHERE articles.id = 5` = 記事 1 件だけを取りたい
- 用途: 記事詳細画面、管理画面の確認画面など
- 一覧との違い: id を指定して 1 件だけ取る

### 20:27〜20:34
FAQ 一覧っぽい SQL を読む

```sql
SELECT faqs.question, users.name, faqs.created_at
FROM faqs
JOIN users ON faqs.user_id = users.id
ORDER BY faqs.created_at DESC
LIMIT 20;
```

見るポイント:
- `FROM faqs` なので FAQ が主役
- 表示したいもの: 質問文、作成者名、作成日時
- 用途: FAQ 一覧、管理画面の新しい FAQ 一覧
- `JOIN` が必要な理由: 作成者名は `users` にあるから

### 20:34〜20:39
カテゴリで絞る記事一覧を読む

```sql
SELECT articles.title, categories.name, articles.published_at
FROM articles
JOIN categories ON articles.category_id = categories.id
WHERE categories.name = 'お知らせ'
ORDER BY articles.published_at DESC;
```

見るポイント:
- `WHERE categories.name = 'お知らせ'` = カテゴリ名で絞り込む
- `WHERE` は `articles` の列だけでなく、JOIN 先の列にも使える
- 用途: お知らせカテゴリの記事一覧

### 20:39〜20:42
自分のアプリに当てはめるミニ演習
- 「公開済み記事の新着 10 件を見たい」
  - `articles` を土台
  - `WHERE status = 'published'`
  - `ORDER BY published_at DESC`
  - `LIMIT 10`

- 「記事 id = 5 の詳細を見たい」
  - `WHERE articles.id = 5`

- 「FAQ を新しい順で 20 件見たい」
  - `faqs` を土台
  - `ORDER BY faqs.created_at DESC`
  - `LIMIT 20`

- 「お知らせカテゴリの記事一覧を見たい」
  - `articles` と `categories` を JOIN
  - `WHERE categories.name = 'お知らせ'`

### 20:42〜20:45
まとめ
- 一覧用 SQL と詳細用 SQL の違い
- `FROM` を見ると何が分かるか
- `WHERE` は JOIN した先の列にも使えるか
- この 3 つを言えるか確認する

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

### 2. 記事 id = 5 の詳細

```sql
SELECT articles.title, articles.body, users.name, categories.name, articles.published_at
FROM articles
JOIN users ON articles.user_id = users.id
JOIN categories ON articles.category_id = categories.id
WHERE articles.id = 5;
```

### 3. FAQ の新しい順 20 件

```sql
SELECT faqs.question, users.name, faqs.created_at
FROM faqs
JOIN users ON faqs.user_id = users.id
ORDER BY faqs.created_at DESC
LIMIT 20;
```

### 4. お知らせカテゴリの記事一覧

```sql
SELECT articles.title, categories.name, articles.published_at
FROM articles
JOIN categories ON articles.category_id = categories.id
WHERE categories.name = 'お知らせ'
ORDER BY articles.published_at DESC;
```

## メモ
- 一覧用 SQL は複数件を取る
- 詳細用 SQL は 1 件を取る
- `FROM articles` なら記事が主役
- `FROM faqs` なら FAQ が主役
- `JOIN` が必要なのは、表示したい値が別テーブルにあるから
- `WHERE articles.status = 'published'` で公開済みだけに絞れる
- `WHERE categories.name = 'お知らせ'` のように JOIN 先の列でも絞れる
- `WHERE id = ...` は 1 件取得によく使う

## 理解チェック
- `WHERE articles.id = 5` はどんな用途の SQL っぽいか
- `FROM faqs` を見ると、何が主役の SQL だと分かるか
- なぜ記事一覧で `users` や `categories` を JOIN するのか
- `WHERE categories.name = 'お知らせ'` は何をしているか
- 次の SQL はどんな画面のためのものっぽいか

```sql
SELECT faqs.question, users.name, faqs.created_at
FROM faqs
JOIN users ON faqs.user_id = users.id
ORDER BY faqs.created_at DESC
LIMIT 20;
```

## 終了条件
- SQL を見て「一覧用」「詳細用」の違いを言える
- `FROM` を見て、どのテーブルが主役か分かる
- `WHERE id = ...` を見て 1 件取得だと読める
- `WHERE status = ...` や `WHERE categories.name = ...` を見て絞り込みの意味が分かる
- 自分のアプリでありそうな SQL を日本語で説明できる