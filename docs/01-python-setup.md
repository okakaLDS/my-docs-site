# 1. Python環境の準備

MkDocsはPython製のツールです。まずPythonが使える状態にします。

## 1-1. すでに入っているか確認する

ターミナル（PowerShellまたはコマンドプロンプト）を開き、次を実行します。

```bash
python --version
```

- バージョン番号（例: `Python 3.12.10`）が表示されれば、インストール済みです。手順1-3へ進んでください。
- 「Pythonが見つかりません。Microsoft Storeから〜」のようなメッセージが出る場合は、未インストールです（Windowsのスタブ機能）。次に進みます。

## 1-2. Pythonをインストールする

Windowsには `winget` というパッケージ管理コマンドが標準で入っているので、これを使います。

```bash
winget install -e --id Python.Python.3.12 --source winget --accept-package-agreements --accept-source-agreements
```

- `-e` : 完全一致するパッケージのみ対象にする（似た名前のパッケージと混同しない）
- `--accept-package-agreements` / `--accept-source-agreements` : ライセンス同意を自動で進める（対話プロンプトで止まらないようにする）

インストールには1〜2分かかります。完了の合図は `Successfully installed` です。

## 1-3. インストール後の確認

**新しいターミナルを開いてから**（PATHの再読み込みのため）次を実行します。

```bash
python --version
pip --version
```

正常な出力例：

```
Python 3.12.10
pip 25.0.1 from C:\Users\xxx\AppData\Local\Programs\Python\Python312\Lib\site-packages\pip (python 3.12)
```

## トラブルシューティング

??? note "`python` が見つからないと出る（インストール後も）"
    PATH環境変数が新しいターミナルに反映されていないことが多いです。
    一度ターミナルを完全に閉じて、新しく開き直してください。
    それでも直らない場合はPCの再起動を試してください。

??? note "Microsoft Storeのスタブが反応してインストールページが開く"
    `python` という名前のコマンドが、Windows標準の「ストアへの誘導用ダミー」を指していることがあります。
    上記の `winget install` を実行すれば正規のPythonに置き換わるので、問題ありません。
