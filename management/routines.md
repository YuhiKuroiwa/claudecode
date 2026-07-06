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
- **更新日**:
  - 2026-07-05: soccer/olise部門を追加、3ベース構成に変更
  - 2026-07-06: 初回自動実行(2026-07-06 08:06 JST)が60分でタイムアウトし、push失敗(コミットなし)。原因は歯止め(検索回数上限・サブエージェント禁止)がプロンプトに無かったため深追いしたと推定。プロンプトに「サブエージェント禁止」「1ベースあたりWebSearch/WebFetch計8回程度まで」「完走優先」を追加
- **備考**: 無人実行のため、許可ツールに含まれる操作は確認プロンプトなしに自動実行される

## コンテスト発見部門(手動実行のみ、自動ルーティンなし)

- `departments/contests/instructions.md` に従い、Kaggle・DrivenDataの募集中コンペを調査する部門
- Kaggleの調査には公式CLI(`kaggle competitions list`)を使用しており、ローカル環境変数 `KAGGLE_API_TOKEN` が必要
- このトークンをクラウドのルーティン実行環境に安全に渡す方法が(2026-07-05時点で)確認できなかったため、**自動ルーティンには組み込まず、必要な時に手動で実行する**運用とした
- 手動実行時は、ローカルの `claude` CLI(このPC)で instructions.md に従って調査を依頼する

## 将棋大会情報部門(手動実行のみ、自動ルーティンなし)

- `departments/shogi/instructions.md` に従い、関東圏(東京・千葉県西部・埼玉県南部・神奈川県北部)の対人将棋大会・定例会/将棋カフェを調査する部門
- 大会情報は時間帯によって変化が緩やかなため、当面は自動化せず必要な時に手動実行する運用とした
