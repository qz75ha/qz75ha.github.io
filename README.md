# qz75ha.github.io

個人ポートフォリオ & 技術ブログ。[https://qz75ha.github.io](https://qz75ha.github.io)

## Tech Stack

- [Astro](https://astro.build/) — 静的サイトジェネレーター
- TypeScript
- GitHub Actions — push で自動デプロイ
- GitHub Pages — ホスティング

## Project Structure

```
src/
├── components/       # Header, Footer など共通コンポーネント
├── content/
│   ├── blog/         # ブログ記事 (.md / .mdx)
│   └── projects/     # 制作物 (.md)
├── layouts/
│   └── BlogPost.astro
├── pages/
│   ├── index.astro   # トップページ
│   ├── about.astro   # About
│   ├── blog/         # ブログ一覧・記事ページ
│   └── projects/     # 制作物一覧・詳細ページ
└── styles/
    └── global.css
```

## Content の追加方法

### ブログ記事

`src/content/blog/` に `.md` または `.mdx` ファイルを追加する。

```md
---
title: '記事タイトル'
description: '概要'
pubDate: '2024-06-01'
heroImage: './image.png'   # 任意
---

本文...
```

### 制作物

`src/content/projects/` に `.md` ファイルを追加する。

```md
---
title: 'プロジェクト名'
description: '一言説明'
pubDate: '2024-06-01'
tags: ['Next.js', 'TypeScript']
url: 'https://...'        # 任意
github: 'https://...'    # 任意
---

詳細説明...
```

## Commands

```sh
npm install        # 依存関係インストール
npm run dev        # 開発サーバー起動 (localhost:4321)
npm run build      # 本番ビルド → dist/
npm run preview    # ビルド結果をローカルでプレビュー
```

## Deploy

`main` ブランチへの push で GitHub Actions が自動実行され、GitHub Pages にデプロイされる。
