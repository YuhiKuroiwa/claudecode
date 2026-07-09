# claudecode

「会社」のような部門構成で、日々の調査・執筆をClaude Codeに行わせるプロジェクト。

## 構成
- `departments/` : 各部門。部門ごとに `instructions.md`(職務規定)と成果物(`reports/`や`posts/`)を持つ
  - `news/it`, `news/business` : 毎朝自動実行(ニュース調査)
  - `soccer/players/olise` : 毎朝自動実行(選手ウォッチ)
  - `contests` : 手動実行(Kaggle/DrivenDataのコンペ発見)
  - `shogi` : 手動実行(関東圏の将棋大会・定例会情報)
  - `blog` : 手動実行(noteに投稿する解説記事の作成)
  - `editorial` : blog部門の記事を校閲(最大3往復、成果物ファイルは持たない)
- `management/logs/` : 各部門の実行ログ(「◯◯部門が△△を作成しました」)
- `management/routines.md` : 自動実行ルーティンの設定内容・変更履歴(claude.ai側の設定はここに無いので記録用)

## 日々のやり取りの流れ
- **朝の挨拶(「こんにちは」等)を受けたら**: `git fetch` → `origin/claude/daily-reports` を `main` にmerge → push。その後、「ブログを更新したい場合は『今日のブログを更新しましょう』と言ってください」の一言を添える
  - 自動ルーティンは `main` に直接pushできないため、`claude/daily-reports` ブランチに溜まる。人間(またはこの会話)が定期的にmergeする必要がある
- **「今日のブログを更新しましょう」と言われたら**: その日の `departments/news/it` と `departments/news/business` のレポート内「ブログ候補」を確認し、おすすめ1つ+他の候補(質問ツールで自由入力も選べる形)を提示する。選ばれたトピックで `departments/blog/instructions.md` → `departments/editorial/instructions.md` の順に実行し、記事を確定する。**投稿(noteへのアップロード)は都度ユーザーが手動で行う**

## 注意事項
- Kaggle部門はローカル環境変数 `KAGGLE_API_TOKEN` を使う(`kaggle` CLI)。クラウドの自動ルーティンでは使えないため手動実行専用
- リポジトリはPublic(中身に機密情報が無いことを確認済み)
