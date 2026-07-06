# 自動実行ルーティン一覧

このリポジトリ自体には自動実行の設定(スケジュール本体)は含まれていません。実行タイミング・使用モデル・使用ツール・起動プロンプトは claude.ai の「Routines」機能側にサーバー保存されています。ここでは、その設定内容を後から追えるように記録しておきます。

## 毎朝調査(ニュースIT・ビジネス・Olise)

- **ルーティンID**: `trig_01KBQsgYEEaT5JYGimRmqits`
- **管理画面**: https://claude.ai/code/routines/trig_01KBQsgYEEaT5JYGimRmqits
- **スケジュール**: 毎日 UTC 23:00 (= 日本時間 8:00)
- **cron式**: `0 23 * * *` (UTC基準)
- **対象リポジトリ**: https://github.com/YuhiKuroiwa/claudecode (main ブランチ)
- **モデル**: claude-sonnet-5
- **許可ツール**: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch
- **実行内容の要約**: 1回の実行で以下3つのベース(担当分野)を順番に調査する
  - `departments/news/it/instructions.md` の規定に従い、IT・テクノロジーニュースを調査 → `departments/news/it/reports/YYYY-MM/YYYY-MM-DD.md`
  - `departments/news/business/instructions.md` の規定に従い、ビジネスニュースを調査 → `departments/news/business/reports/YYYY-MM/YYYY-MM-DD.md`
  - `departments/soccer/players/olise/instructions.md` の規定に従い、Michael Oliseの次戦予定・新着トピックを調査 → `departments/soccer/players/olise/reports/YYYY-MM/YYYY-MM-DD.md`
  - それぞれの作成結果を `management/logs/YYYY-MM/YYYY-MM-DD.md` に追記し、まとめて git commit & push する
- **作成日**: 2026-07-05
- **push先ブランチ**: `claude/daily-reports`(`main`への直接pushは権限エラーになるため)。`main`への取り込みは人間側(ローカルの`git merge`)で定期的に行う
- **更新日**:
  - 2026-07-05: soccer/olise部門を追加、3ベース構成に変更
  - 2026-07-06: 初回自動実行(2026-07-06 08:06 JST)が失敗。実行ログを見るとタイムアウトではなく**リポジトリのclone時点でGitHub認証エラー**だった(当時リポジトリがPrivateで、クラウド環境の書き込み権限が未設定だったため)
  - 2026-07-06: リポジトリをPublicに変更(中身に機密情報が無いことを確認済み)。再実行するとclone/検索/レポート作成/コミットまでは成功したが、今度は**push時に403エラー**(`main`への直接pushは`claude/`接頭辞ブランチ以外禁止という仕様、および書き込み権限そのものが未設定だったことが複合)
  - 2026-07-06: 根本解決のため `gh` CLI・Node.js・Claude Code CLIをローカルにインストールし、GitHub Personal Access Tokenで`gh auth login`後、`claude` CLIで`/web-setup`を実行してGitHubの書き込み権限をclaude.aiアカウントに同期。プロンプトも push先を`claude/daily-reports`ブランチに変更。これで自動実行→push成功を確認(2026-07-06 実行分より正常動作)
- **備考**: 無人実行のため、許可ツールに含まれる操作は確認プロンプトなしに自動実行される

## コンテスト発見部門(手動実行のみ、自動ルーティンなし)

- `departments/contests/instructions.md` に従い、Kaggle・DrivenDataの募集中コンペを調査する部門
- Kaggleの調査には公式CLI(`kaggle competitions list`)を使用しており、ローカル環境変数 `KAGGLE_API_TOKEN` が必要
- このトークンをクラウドのルーティン実行環境に安全に渡す方法が(2026-07-05時点で)確認できなかったため、**自動ルーティンには組み込まず、必要な時に手動で実行する**運用とした
- 手動実行時は、ローカルの `claude` CLI(このPC)で instructions.md に従って調査を依頼する

## 将棋大会情報部門(手動実行のみ、自動ルーティンなし)

- `departments/shogi/instructions.md` に従い、関東圏(東京・千葉県西部・埼玉県南部・神奈川県北部)の対人将棋大会・定例会/将棋カフェを調査する部門
- 大会情報は時間帯によって変化が緩やかなため、当面は自動化せず必要な時に手動実行する運用とした

## ブログ執筆部門(手動実行のみ、自動ルーティンなし)

- `departments/blog/instructions.md` に従い、指定されたトピック(news部門のレポート内の「ブログ候補」または自由テーマ)を深掘り調査し、note投稿用の解説記事を作成する部門
- 文体: です・ます調の平易な解説記事、2,000〜3,000字程度
- 出力先: `departments/blog/posts/YYYY-MM/YYYY-MM-DD-スラッグ.md`
- ユーザーからの都度の指示で実行するため自動ルーティンには含めない
- 関連: `departments/news/it`・`departments/news/business` のレポート末尾に「ブログ候補」セクションを追加済み(2026-07-06〜)
- 構成: 導入 → 起源・由来 → 今回何が起きたか(解説) → 現代の使われ方・雑学 → まとめ → 参考
- 記事確定前に `departments/editorial` の校閲を経る(下記参照)

## 校閲部門(手動実行のみ、自動ルーティンなし、成果物ファイルなし)

- `departments/editorial/instructions.md` に従い、他部門(まずはブログ部門のみ)の文章を読みやすさ・トーン・面白みの観点で校閲する部門
- ブログ記事の場合は特にユーモア性・読み物としての面白みも見る
- 校閲→修正のやり取りは最大3往復まで。それ以降の軽微な指摘は許容して確定する
- 校閲部門自体はファイルを持たず、対象ファイルを直接修正して確定させる形
