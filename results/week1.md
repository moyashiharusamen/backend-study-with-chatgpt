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