# リリース手順（GitHub Releases + 更新通知）

配布は **GitHub Releases** で行います。各バージョンのZXPは Release `v<version>` の
アセットとして公開し、更新通知は `mojibunkai/latest.json`（署名付き）が指し示します。
リポジトリ本体にはバイナリを残しません（履歴にのみ残ります）。

## 一度だけの設定

GitHub の **Settings → Secrets and variables → Actions → New repository secret**
- `ADMIN_TOKEN` … ライセンスサーバー（Worker）の管理トークン

（WorkerのURLが変わったら **Variables** に `WORKER_URL` を追加して上書き。）

## 毎回のリリース（担当：Claude）

1. **ソース側**（`adobe-plugins`）でバージョンを上げ、署名・難読化ZXPをビルドし、
   このリポジトリへ `mojibunkai/MojiBunkai-<version>.zxp` を一時的にコミット/push
   （`illustrator/MojiBunkai/tools/release.sh <version>` が実行）。
2. **Actions → Publish update → Run workflow** に `version`（例 `1.3.0`）と `notes` を入力して実行。
   ワークフローが自動で：
   - Release `v<version>` を作成し、ZXPをアセットとして添付
   - `latest.json` を **Releaseアセットのurl** で署名（Worker経由）してコミット
   - リポジトリ本体から一時ZXPを削除
3. 既存ユーザーには次回起動時に「<version> が利用できます」の通知が出ます。

## 配布URL

- ダウンロード：`https://github.com/testtbro/moji-bunkai-dist/releases`（各Releaseのアセット）
- 更新通知（プラグインが参照）：`https://raw.githubusercontent.com/testtbro/moji-bunkai-dist/main/mojibunkai/latest.json`

> 秘密鍵は Worker の中だけ、`ADMIN_TOKEN` は GitHub Secrets の中だけで、外に出ません。
