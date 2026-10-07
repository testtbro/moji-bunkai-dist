# リリース手順（更新の公開 → 更新通知）

このリポジトリは配布物（署名済みZXP）と更新通知（`mojibunkai/latest.json`）を置く場所です。
ソースは非公開リポジトリ `adobe-plugins` にあり、ビルドはそちらで行います。

## 一度だけの設定

GitHub の **Settings → Secrets and variables → Actions → New repository secret**
- `ADMIN_TOKEN` … ライセンスサーバー（Worker）の管理トークン

（WorkerのURLが変わったら、同画面の **Variables** に `WORKER_URL` を追加して上書きできます。）

## 毎回のリリース（担当：Claude がコミット/実行まで行う）

1. **ソース側**（`adobe-plugins`）でバージョンを上げ、署名・難読化済みZXPをビルド
   （`illustrator/MojiBunkai/tools/release.sh <version>` が、ビルドしてこのリポジトリへ
   `mojibunkai/MojiBunkai-<version>.zxp` をコミット/pushするところまで行います）。
2. **更新通知を公開**：GitHub の **Actions → Publish update → Run workflow** で
   `version`（例 `1.3.0`）と任意の `notes` を入力して実行。
   - これが Worker に `latest.json` を署名させ、`mojibunkai/latest.json` をコミットします。
   - 秘密鍵は Worker の中だけ、`ADMIN_TOKEN` は GitHub Secrets の中だけで、外に出ません。
3. 既存ユーザーには次回起動時に「<version> が利用できます」の通知が出ます。

> ZXP が未コミットのバージョンを指定すると、ワークフローはエラーで止まります（先に手順1）。
