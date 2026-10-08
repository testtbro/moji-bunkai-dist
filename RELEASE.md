# リリース手順（GitHub Releases + 更新通知）

配布は **GitHub Releases** で行います。各バージョンのZXPは Release `v<version>` の
アセットとして公開し、更新通知は `mojibunkai/latest.json`（署名付き）が指し示します。
リポジトリ本体にはバイナリを残しません（履歴にのみ残ります）。

## 一度だけの設定

GitHub の **Settings → Environments → New environment** で `release` を作成し、
- **Required reviewers** にオーナー（自分）を追加して保存
- 同じ画面の **Environment secrets** に `ADMIN_TOKEN`（ライセンスサーバー Worker の管理トークン）を追加

リポジトリ全体の Secret（Settings → Secrets and variables → Actions）に `ADMIN_TOKEN` が残っていれば削除します。
これで、ワークフローはオーナーが承認した実行でしか管理トークンを使えません。

（WorkerのURLが変わったら **Variables** に `WORKER_URL` を追加して上書き。）

## 毎回のリリース（担当：Claude）

1. **ソース側**（`adobe-plugins`）でバージョンを上げ、署名・難読化ZXPをビルドし、
   このリポジトリへ `mojibunkai/MojiBunkai-<version>.zxp` を一時的にコミット/push
   （`illustrator/MojiBunkai/tools/release.sh <version>` が実行し、ZXP の SHA-256 を表示）。
2. **Actions → Publish update → Run workflow** に `version`（例 `1.3.0`）、`sha256`（手順1の値）、
   `notes` を入力して実行し、**オーナーが Review deployments → Approve で承認**。
   ワークフローが自動で：
   - ZXP の SHA-256 が入力値と一致することを確認
   - Release `v<version>` を作成し、ZXPをアセットとして添付
   - `latest.json` を **Releaseアセットのurl と SHA-256** で署名（Worker経由）してコミット
   - リポジトリ本体から一時ZXPを削除
3. 既存ユーザーには次回起動時に「<version> が利用できます」の通知が出ます。

`mojibunkai/config.json`（接続先URL）は署名付きです。URL を変えたときだけ、
実行時に `update_endpoints` にチェックを入れて再署名します（未チェックで URL が変わっていると停止します）。

## 配布URL

- ダウンロード：`https://github.com/testtbro/moji-bunkai-dist/releases`（各Releaseのアセット）
- 更新通知（プラグインが参照）：`https://raw.githubusercontent.com/testtbro/moji-bunkai-dist/main/mojibunkai/latest.json`

> 秘密鍵は Worker の中だけ、`ADMIN_TOKEN` は `release` 環境の Secret の中だけで、外に出ません。
