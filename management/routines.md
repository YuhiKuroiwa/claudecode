# 自動実行ルーティン一覧

このリポジトリ自体には自動実行の設定(スケジュール本体)は含まれていません。実行タイミング・使用モデル・使用ツール・起動プロンプトは claude.ai の「Routines」機能側にサーバー保存されています。ここでは、その設定内容を後から追えるように記録しておきます。

## 投資入門シリーズ 毎日自動執筆 ― 稼働中(2026-09-06登録)

- **状態**: 有効(`enabled: true`)
- **ルーティンID**: `trig_01MkVcaKBTpWVc9R4csBwqBS`
- **管理画面**: https://claude.ai/code/routines/trig_01MkVcaKBTpWVc9R4csBwqBS
- **スケジュール**: 毎日 UTC 22:00(= 日本時間 朝7:00)
- **cron式**: `0 22 * * *`(UTC基準)
- **対象リポジトリ**: https://github.com/YuhiKuroiwa/claudecode(main ブランチ)、環境は既存の`env_01TpL4d1TiJfs5YUCV7ChKmS`を再利用
- **モデル**: claude-sonnet-5
- **許可ツール**: Bash, Read, Write, Edit, Glob, Grep, WebSearch, WebFetch(Agent/サブエージェントは使わない方針)
- **push先ブランチ**: `claude/daily-reports`(`main`への直接pushは権限エラーになるため)。`main`への取り込みは人間側(ローカルの`git merge`)で定期的に行う
- **実行内容の要約**: 1回の実行で以下を1話分通しで行う
  1. `departments/blog/series-investing-start/roadmap.md` を確認し、次に書く回を判断する
  2. `departments/research/investing/instructions.md` に従って調査メモを作成
  3. `departments/blog/series-investing-start/instructions.md` に従って記事を執筆
  4. `departments/editorial/instructions.md` に従って校閲(最大3往復)
  5. `roadmap.md`を更新し、`management/logs/YYYY-MM/YYYY-MM-DD.md`に実行結果を追記してcommit & push
- **公開について**: このルーティンは下書き作成までを行う。noteへの実際の投稿はユーザーが手動で行う(自動投稿はしない方針、詳細は`CLAUDE.md`参照)
- **前回(旧構成)からの申し送り**: 以前はニュース速報系の部門で「毎日実行だが確認が追いつかず一時停止」という経緯があった。今回は下書きの確認・note投稿をまとめて数日分行ってもよい運用にすることで、同じ理由での停止を避ける
