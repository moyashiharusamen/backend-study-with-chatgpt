# Week2 Day1（2026-04-13 / 4月13日（月））
テーマ: CREATE TABLE の最小理解

## この日の位置づけ
- Week1 で学んだ「テーブル / カラム / 主キー / 外部キー」の概念を、実際の SQL の形で見る日
- 構文暗記ではなく、CREATE TABLE 文を役割ごとに読めるようにする

## 到達目標
- CREATE TABLE articles (...) を見て「articles テーブルを作っている」と言える
- id, title, body, created_at などの列の意味をざっくり説明できる
- integer, varchar, text, timestamp の用途を大まかに理解する
- NOT NULL を「空を許可しない」と説明できる
- articles テーブルに必要そうな列を3〜5個書ける

## 進め方

### 20:00〜20:05
導入
- テーブル = データを入れる箱
- カラム = 箱の中の項目
- CREATE TABLE = 箱を作る命令

### 20:05〜20:15
CREATE TABLE 文の全体像を見る
- CREATE TABLE articles は articles テーブルを作る
- () の中に列定義を書く
- 1行 = 1つの列定義として読む

サンプル:
```sql
CREATE TABLE articles (
  id integer,
  title varchar(255),
  body text,
  created_at timestamp
);