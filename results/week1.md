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