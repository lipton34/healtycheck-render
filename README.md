# healtycheck-render

## GitHub Actionsの監視は停止済み

2026-10-29、cron-job.orgへの移行に伴い、GitHub Actionsのヘルスチェック用ワークフローを削除しました。このリポジトリから定期・push・手動のヘルスチェックは実行しません。過去のファイルと実行記録はGit履歴・Actions履歴に残ります。

## 現在の休止対策

cron-job.orgで `https://saisoku.onrender.com/api/health` を5分おき・毎日24時間、GETするジョブを有効にしています。利用者が自動実行2回の成功を確認済みです。長時間の安定稼働は別途確認が必要です。

cron-job.orgのジョブやアプリ本体、DB、Push通知設定は今回変更していません。実行結果はcron-job.orgのHistoryで確認します。停止・再開は同サービスのEnable jobで管理します。

## 移行の理由

GitHub側は導入時のpushチェックは成功したものの、その後約1時間40分の間にschedule実行が確認できず、休止対策として機能していませんでした。cronの再登録も試みましたが、GitHubの定期実行だけに依存する構成は終了しました。

この公開リポジトリにはアプリ本体のソースコード、利用者データ、接続情報、秘密鍵は保存しません。Saisoku本体はPrivateです。
