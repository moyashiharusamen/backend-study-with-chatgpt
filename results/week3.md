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