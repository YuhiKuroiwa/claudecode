# 自動実行ルーティン一覧

このリポジトリ自体には自動実行の設定(スケジュール本体)は含まれていません。実行タイミング・使用モデル・使用ツール・起動プロンプトは claude.ai の「Routines」機能側にサーバー保存されています。ここでは、その設定内容を後から追えるように記録しておきます。

## ニュース部門 毎朝調査

- **ルーティンID**: `trig_01KBQsgYEEaT5JYGimRmqits`
- **管理画面**: https://claude.ai/code/routines/trig_01KBQsgYEEaT5JYGimRmqits
- **スケジュール**: 毎日 UTC 23:00 (= 日本時間 8:00)
- **cron式**: `0 23 * * *` (UTC基準)
- **対象リポジトリ**: https://github.com/YuhiKuroiwa/claudecode (main ブランチ)
- **モデル**: claude-sonnet-5
- **許可ツール**: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch
- **実行内容の要約**: `departments/news/instructions.md` の規定に従い、直近24〜48時間のIT・テクノロジーニュースを調査。`departments/news/reports/YYYY-MM-DD.md` を作成し、`management/logs/YYYY-MM-DD.md` に実行ログを追記後、git commit & push する
- **作成日**: 2026-07-05
- **備考**: 無人実行のため、許可ツールに含まれる操作は確認プロンプトなしに自動実行される
