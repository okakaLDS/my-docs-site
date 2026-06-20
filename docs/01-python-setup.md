# 1. Python環境の準備

MkDocsはPython製のツールなので、まずPythonをインストールします。

このPCにはPythonが入っていなかった（Microsoft Storeのスタブのみ）ため、
wingetでインストールしました。

```bash
winget install -e --id Python.Python.3.12 --source winget \
  --accept-package-agreements --accept-source-agreements
```

インストール後、確認:

```bash
python --version
pip --version
```

```
Python 3.12.10
pip 25.0.1
```
