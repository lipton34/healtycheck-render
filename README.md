# healtycheck-render

Saisokuの公開ヘルスチェックURLを確認するための監視専用リポジトリです。アプリ本体のソースコード、利用者データ、接続情報、秘密鍵は保存しません。

## 動作

- GitHub Actionsの標準Ubuntuランナーから `https://saisoku.onrender.com/api/health` を5分おき（毎時0・5・10…55分）にGETする設定です。
- HTTP成功とJSONの `ok: true` を確認します。起動待ちのHTMLは成功扱いしません。
- 1回70秒、最大2回の再試行、ジョブ全体は4分で打ち切ります。
- mainへのワークフロー導入・変更時にも実行します。Actionsの「Run workflow」から手動実行できます。
- checkout、シークレット、書込権限、依存インストール、キャッシュ、成果物は使用しません。
- この公開リポジトリだけで動き、フォークや非公開化後はジョブを実行しません。

## 費用と制約

公開リポジトリの標準GitHubホストランナーを使うため、Actionsの実行は無料です。Render側のFree稼働時間・通信量等は従来どおり消費します。

GitHubの定期実行には遅延・欠落があり、常時稼働や予定通知の定刻到着を保証しません。60日間リポジトリに活動がないと定期実行が自動停止するため、GitHubからの通知と実行履歴を確認し、必要に応じて再有効化してください。自動コミットによる回避は行いません。

## 確認・停止・再開

[Actionsの実行履歴](https://github.com/lipton34/healtycheck-render/actions/workflows/render-health-check.yml) で成功・失敗と実行間隔を確認します。停止は「Disable workflow」、再開は「Enable workflow」です。

このチェックは認証やDBアクセスを行わず、Push通知も送りません。チェック成功は通知配信やDB接続の正常性を保証するものではありません。

## 定期実行の確認

導入時のpush実行成功だけでは定期監視の稼働確認になりません。実行履歴でイベントが `schedule` の実行を確認し、15分以上の空白がないことを確認してください。

2026-10-28の確認では導入後約1時間40分の間、push実行1件だけでschedule実行がなく、休止対策が機能していませんでした。原因は未確定です。定期設定を標準的な5分間隔へ変更して再登録を試みています。scheduleの実行と継続間隔を確認するまでは、休止を防げている扱いにしません。GitHub Actionsの定期実行は遅延・欠落があるため、通知用途の安定稼働が必要なら専用の外部監視を使用してください。

## 公式資料

- [GitHub Actionsの料金](https://docs.github.com/en/billing/concepts/product-billing/github-actions)
- [定期実行の制約](https://docs.github.com/en/actions/reference/workflows-and-actions/events-that-trigger-workflows#schedule)
- [Render Freeの休止と利用枠](https://render.com/docs/free)
