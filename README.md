# 文字分解 Moji Bunkai — 配布リポジトリ

Illustrator プラグイン「文字分解 Moji Bunkai」の配布・自動アップデート用の公開リポジトリです。
（ソースは非公開リポジトリで管理しています。ここには配布物だけを置きます。）

## ダウンロード / インストール

`mojibunkai/MojiBunkai-<version>.zxp` をダウンロードし、ZXP Installer か Adobe UPIA でインストールしてください（Mac / Windows 共通）。

## ファイル

| パス | 役割 |
|---|---|
| `mojibunkai/MojiBunkai-<version>.zxp` | プラグイン本体（署名済み） |
| `mojibunkai/latest.json` | 署名付きアップデート情報（パネルが参照） |
| `mojibunkai/config.json` | サーバーURL等（パネルが起動時に参照） |

## 重要：デプロイ後に1か所だけ編集

ライセンスサーバー（Cloudflare Worker）をデプロイしたら、`mojibunkai/config.json` の
`license_api` を、払い出された Worker の URL（末尾 `/activate`）に書き換えてコミットしてください。
プラグインの再ビルドは不要です。
