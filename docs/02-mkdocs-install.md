# 2. mkdocsのインストールとサイト作成

## インストール

見栄えの良いMaterialテーマも一緒に入れます。

```bash
pip install mkdocs mkdocs-material
```

## サンプルサイトの作成

```bash
mkdocs new my-docs-site
```

以下のファイル構成が作られます。

```
my-docs-site/
├── docs/
│   └── index.md
└── mkdocs.yml
```

## テーマと言語の設定

`mkdocs.yml` にMaterialテーマと日本語設定を追加しました。

```yaml
site_name: My Docs
site_url: https://okakaLDS.github.io/my-docs-site/
theme:
  name: material
  language: ja
nav:
  - ホーム: index.md
```

## ビルド確認

```bash
mkdocs build
```

エラーなくサイトが `site/` フォルダに生成されればOKです。
