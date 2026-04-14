# Week2 Day1（2026-04-13 / 4月13日（月））
テーマ: CREATE TABLE の最小理解

## この日の位置づけ
- Week1 で学んだ「テーブル / カラム / 主キー / 外部キー」の概念を、実際の SQL の形で見る日
- 構文暗記ではなく、CREATE TABLE 文を役割ごとに読めるようにする

## 到達目標
- `CREATE TABLE articles (...)` を見て「articles テーブルを作っている」と言える
- `id`, `title`, `body`, `created_at` などの列の意味をざっくり説明できる
- `integer`, `varchar`, `text`, `timestamp` の用途を大まかに理解する
- `NOT NULL` を「空を許可しない」と説明できる
- `articles` テーブルに必要そうな列を 3〜5 個書ける

## 進め方

### 20:00〜20:05
導入
- テーブル = データを入れる箱
- カラム = 箱の中の項目
- `CREATE TABLE` = 箱を作る命令

### 20:05〜20:15
CREATE TABLE 文の全体像を見る
- `CREATE TABLE articles` は `articles` テーブルを作る
- `()` の中に列定義を書く
- 1 行 = 1 つの列定義として読む

サンプル:

```sql
CREATE TABLE articles (
  id integer,
  title varchar(255),
  body text,
  created_at timestamp
);
```

### 20:15〜20:25
型の最小理解
- `integer` = 整数
- `varchar(255)` = 短い文字列
- `text` = 長文
- `timestamp` = 日時

### 20:25〜20:35
NOT NULL を理解する
- `NOT NULL` = 空を許可しない
- 型だけでなく、列にルールをつけられると理解する

サンプル:

```sql
CREATE TABLE articles (
  id integer NOT NULL,
  title varchar(255) NOT NULL,
  body text,
  created_at timestamp NOT NULL
);
```

### 20:35〜20:42
自分のアプリに当てはめる
`articles` に必要そうな列を考える
- `id`
- `title`
- `body`
- `status`
- `published_at`
- `user_id`
- `category_id`
- `created_at`
- `updated_at`

各列の意味を日本語でメモする
- `id` = 記事 1 件を区別する番号
- `title` = 記事タイトル
- `body` = 記事本文
- `status` = 下書きか公開か
- `published_at` = 公開日時
- `user_id` = 誰が作った記事か
- `category_id` = どのカテゴリか

### 20:42〜20:45
まとめ
- `CREATE TABLE` は何をする文か
- `NOT NULL` は何か
- `articles` に必要そうな列を 3 つ以上言えるか確認する

## この日に使うサンプル SQL

```sql
CREATE TABLE articles (
  id integer NOT NULL,
  title varchar(255) NOT NULL,
  body text,
  status varchar(50) NOT NULL,
  published_at timestamp,
  created_at timestamp NOT NULL,
  updated_at timestamp NOT NULL
);
```

## 理解チェック
- `CREATE TABLE articles (...)` は何をしている文か
- `title varchar(255) NOT NULL` は何を意味するか
- `body text` は何を入れる列か
- `published_at timestamp` は何を入れる列か
- `articles` テーブルに必要そうな列を 3 つ以上言えるか

## 終了条件
- `CREATE TABLE` を見て「テーブル作成の SQL」と分かる
- 1 行ずつ見て「列名」と「型」を分けて読める
- `NOT NULL` の意味を説明できる
- `articles` に必要そうな列を挙げられる

# Week2 Day2（2026-04-14 / 4月14日（火））
テーマ: 主キー・外部キーを CREATE TABLE の形で見る

## この日の位置づけ
- Week1 で学んだ主キー・外部キーの概念を、実際のテーブル定義の形で読む日
- 構文暗記ではなく、`id` / `user_id` / `category_id` の役割を区別して読めるようにする

## 到達目標
- `id` を見て「そのテーブルの1件を区別する列」と言える
- `user_id` を見て「users テーブルにつながる列」と言える
- `category_id` を見て「categories テーブルにつながる列」と言える
- `articles` テーブルが `users` と `categories` に属するイメージを説明できる
- `faqs.user_id` を見て「FAQ を作成した user につながる」と言える
- 自分のアプリの 4 テーブルについて、主キーと外部キーの候補を言える

## 進め方

### 20:00〜20:05
導入
- Day1 の復習
- `CREATE TABLE` = テーブルを作る SQL
- 1 行 = 1 つの列定義
- 今日は「つながりを表す列」を見る日

### 20:05〜20:15
主キーをテーブル定義の中で見る

```sql
CREATE TABLE users (
  id integer NOT NULL,
  name varchar(255) NOT NULL,
  email varchar(255) NOT NULL
);
```

- `users.id` は users テーブルの 1 件を区別するための列
- 主キー = そのテーブルの 1 件を区別するもの
- 多くのテーブルでは `id` がその役割を持つ

### 20:15〜20:25
外部キーの元になる列を読む

```sql
CREATE TABLE articles (
  id integer NOT NULL,
  user_id integer NOT NULL,
  category_id integer NOT NULL,
  title varchar(255) NOT NULL,
  body text,
  created_at timestamp NOT NULL
);
```

- `user_id` は users テーブルの誰が作った記事かを表す
- `category_id` は categories テーブルのどのカテゴリかを表す
- 外部キーの列は、別テーブルにつながるための列

### 20:25〜20:32
`id` と `user_id` の違いを理解する
- `id` = 自分自身を区別する番号
- `user_id` = 別テーブルの行を指す番号
- どちらも integer だが、役割が違う

### 20:32〜20:38
`faqs` テーブルにも当てはめる

```sql
CREATE TABLE faqs (
  id integer NOT NULL,
  user_id integer NOT NULL,
  question text NOT NULL,
  answer text NOT NULL,
  created_at timestamp NOT NULL
);
```

- `faqs.id` は FAQ 1 件を区別する
- `faqs.user_id` は FAQ を作成した user につながる

### 20:38〜20:42
自分のアプリの 4 テーブルで整理する
対象:
- users
- articles
- categories
- faqs

考えること:
- 主キーは何か
- 外部キーになりそうな列は何か

整理例:
- `users.id`
- `articles.id`
- `articles.user_id`
- `articles.category_id`
- `categories.id`
- `faqs.id`
- `faqs.user_id`

### 20:42〜20:45
まとめ
- 主キーとは何か
- 外部キーの列とは何か
- `articles.user_id` はどこにつながるか確認する

## この日に使うサンプル SQL

```sql
CREATE TABLE users (
  id integer NOT NULL,
  name varchar(255) NOT NULL,
  email varchar(255) NOT NULL
);
```

```sql
CREATE TABLE articles (
  id integer NOT NULL,
  user_id integer NOT NULL,
  category_id integer NOT NULL,
  title varchar(255) NOT NULL,
  body text,
  created_at timestamp NOT NULL
);
```

```sql
CREATE TABLE faqs (
  id integer NOT NULL,
  user_id integer NOT NULL,
  question text NOT NULL,
  answer text NOT NULL,
  created_at timestamp NOT NULL
);
```

## メモ
- 主キー = そのテーブルの 1 件を区別する列
- 外部キーの列 = 別テーブルにつながるための列
- `articles.user_id` → `users.id`
- `articles.category_id` → `categories.id`
- `faqs.user_id` → `users.id`
- `id` は自分自身を区別する
- `user_id` は別テーブルを指す

## 理解チェック
- 主キーとは何か
- `articles.id` と `articles.user_id` の違いは何か
- `articles.category_id` はどこにつながるか
- `faqs.user_id` は何を表すか
- 外部キーになりそうな列を 2 つ以上言えるか

## 終了条件
- `id` を見て主キーの役割を説明できる
- `user_id` や `category_id` を見て、別テーブルとのつながりを説明できる
- `articles` と `faqs` の外部キー候補を言える
- `id` と `user_id` の役割の違いを説明できる