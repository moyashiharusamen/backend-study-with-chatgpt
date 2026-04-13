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