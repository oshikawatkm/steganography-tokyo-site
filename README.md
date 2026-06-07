# STEGANOGRAPHY TOKYO — Web site

素の HTML / CSS のみ。ビルド工程・依存パッケージ・JavaScript なし。
GitHub Pages にそのまま置けば動きます。ランニングコストは **ドメイン更新料のみ**。

## 構成

```
.
├── index.html            TOP（モック再現）
├── about.html            ABOUT US
├── artworks.html         ART WORKS（TBD）
├── 404.html              Not Found ページ
├── CNAME                 → steganography.tokyo（GitHub Pages 用）
├── favicon.svg
├── .nojekyll             Jekyll 処理をスキップ
└── assets/
    ├── style.css         全ページ共通スタイル
    └── bg.jpg            背景写真（最適化済 / 約140KB）
```

CONTACT は X（https://x.com/Stegano_tokyo21）、EVENT は Luma（https://luma.com/bc0jp8yx）へ直接リンクしています。

## ローカル確認

```bash
cd <このフォルダ>
python3 -m http.server 8000
# → http://localhost:8000
```

## デプロイ手順（GitHub Pages）

1. GitHub で **private リポジトリ**を作成。
2. このフォルダの中身を push。
   ```bash
   git init
   git add .
   git commit -m "init: steganography.tokyo"
   git branch -M main
   git remote add origin git@github.com:<account>/<repo>.git
   git push -u origin main
   ```
3. リポジトリの **Settings → Pages** で
   *Source* を「Deploy from a branch」、ブランチ `main` / フォルダ `/ (root)` に設定して Save。
4. 同じ Pages 画面の **Custom domain** に `steganography.tokyo` を入力して Save。
   （`CNAME` ファイルは同梱済みなので自動で認識されます）
5. DNS（下記）の伝播後、**Enforce HTTPS** にチェック。証明書は Let's Encrypt で自動発行されます。

## DNS 設定（ここが重要）

> GitHub Pages は **DNS をホストしません**（ネームサーバを提供しません）。
> レコードは **ドメインを取得した業者の DNS 管理画面**（お名前.com 等の `.tokyo` レジストラ、または Cloudflare DNS のような無料 DNS）で設定します。
> GitHub 側でやるのは上記 4 の Custom domain 入力だけです。

apex ドメイン（`steganography.tokyo`）に **A レコード 4 本**を作成：

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

IPv6 対応する場合は **AAAA レコード 4 本**も追加（任意・推奨）：

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

`www` も使いたい場合は **CNAME** を 1 本：

```
www  →  <account>.github.io
```

注意点：
- 既存のデフォルト A レコードやワイルドカード（`*`）レコードがあれば削除する。
- 反映には最大 24 時間ほどかかることがある（通常は数分〜数十分）。
- 設定確認： `dig steganography.tokyo +short`

## 編集メモ

- **TBD ページに中身を入れる**：各 `*.html` の `<main class="page">…</main>` を本文に差し替えるだけ。共通の見た目は `assets/style.css` が担当。
- **背景画像の差し替え**：`assets/bg.jpg` を同名で置き換え。右側の黒いグラデーションは CSS（`style.css` の `.stage` 内 `linear-gradient`）で重ねているので、画像自体に焼き込む必要はありません。濃さ・角度はそこで調整。
- **フォント**：Web フォントは未使用。Mac は Helvetica Neue、Windows は Arial、和文は Hiragino / Yu Gothic / Meiryo を自動使用（モックに近い見た目・読み込みコスト 0）。OS 間で字形を完全に揃えたい場合のみ Web フォントの self-host を検討。
- **色・余白**：`style.css` 冒頭の `:root` 変数（`--ink` 文字色、`--solid-bg` ボタン色、`--edge` 余白など）でまとめて調整可能。
