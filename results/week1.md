# Week1 学習メモ

## Day1（2026-04-06）
### テーマ
概念理解

### わかったこと
- 主キー = 1件を区別するためのもの
- 外部キー = 他のテーブルとのつながりを表すもの
- Article belongs_to :user は「1つの記事は1人のユーザーに属する」という意味
- User has_many :articles は「1人のユーザーは複数の記事を持てる」という意味
- articles.user_id のような列で、記事とユーザーを結びつけられる

### まだ曖昧なこと
- 外部キーの「列」と「制約」の違い
- belongs_to と has_many の内部動作

## Day2（2026-04-07）
### テーマ
SELECT と WHERE の基本

### わかったこと
- SELECT は列を選ぶ
- WHERE は行を絞る
- status = 'published' のように条件を書ける
- 文字列はクォートで囲う
- 数値はそのまま書く

### 書いたSQL
SELECT id, title FROM articles;

SELECT id, title, status
FROM articles
WHERE status = 'published';

SELECT id, question, status
FROM faqs
WHERE status = 'draft';

### まだ曖昧なこと
- 文字列にクォートが必要な理由
- * をどこまで使ってよいか
- 複数条件の書き方

## Day3（2026-04-08）
### テーマ
ORDER BY と LIMIT の基本

### わかったこと
- ORDER BY は並び順を決める
- ASC は小さい順、DESC は大きい順
- LIMIT は件数を絞る
- WHERE と ORDER BY と LIMIT は一緒に使える
- id DESC にすると新しいものを上から見やすい

### 書いたSQL
SELECT id, title
FROM articles
ORDER BY id DESC;

SELECT id, title
FROM articles
LIMIT 5;

SELECT id, title, status
FROM articles
WHERE status = 'published'
ORDER BY id DESC
LIMIT 5;

### まだ曖昧なこと
- ORDER BY title のように文字列でも並び替えできるか
- LIMIT を書く位置のルール
- 何を基準に id DESC にすればよいか

## Day4（2026-04-09）
### テーマ
自分のアプリに戻して、最低限のテーブルと列名を整理する

### わかったこと
- 自分のアプリでは users / articles / categories / faqs が土台になる
- 列名は最初から増やしすぎず、最低限から考える
- user_id は「誰が作ったか」を表す
- category_id は「どの分類に属するか」を表す
- DBの外部キーと Rails の belongs_to / has_many は対応している

### 最低限のテーブルと列
users
- id
- name
- email

articles
- id
- title
- body
- status
- user_id
- category_id

categories
- id
- name

faqs
- id
- question
- answer
- status
- user_id

### 関連のメモ
- 1人の user は複数の articles を持てる
- 1つの article は1人の user に属する
- 1つの article は1つの category に属する
- 1つの category には複数の articles が入る
- 1人の user は複数の faqs を持てる
- 1つの faq は1人の user に属する

### Rails側の対応イメージ
- Article belongs_to :user
- Article belongs_to :category
- User has_many :articles
- Category has_many :articles
- Faq belongs_to :user
- User has_many :faqs

### まだ曖昧なこと
- status を文字列で持つか数値で持つか
- body と answer の型をどうするか
- created_at や published_at をどの段階で足すか
- categories と faqs を今後どう広げるか

## Day5（2026-04-10）
### テーマ
INSERT / UPDATE / DELETE の基本

### わかったこと
- INSERT は新しい行を追加する
- UPDATE は既存の行を変更する
- DELETE は既存の行を削除する
- UPDATE と DELETE は WHERE がないと危険
- 最初は id で1件を指定して扱うと分かりやすい

### 書いたSQL
INSERT INTO articles (title, body, status, user_id, category_id)
VALUES ('はじめての記事', '本文です', 'draft', 1, 2);

UPDATE articles
SET status = 'published'
WHERE id = 3;

DELETE FROM faqs
WHERE id = 5;

### まだ曖昧なこと
- INSERT で全部の列を書かないとどうなるか
- UPDATE で複数列を変える書き方
- DELETE と論理削除の違い
- Rails ではどう書くのか