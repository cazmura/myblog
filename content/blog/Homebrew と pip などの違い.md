+++
title = "Homebrew と pip などの違い"
date = "2026-01-01T12:45:57+09:00"
draft = false
tags = ["環境構築", "AIが生成したもの"]
images = []
+++

### 3つのツールの違い

| ツール          | 対象               | インストール先        | 用途                |
| ------------ | ---------------- | -------------- | ----------------- |
| **pip**      | Pythonパッケージ      | プロジェクトまたはシステム  | Pythonライブラリ       |
| **npm**      | JavaScriptパッケージ  | プロジェクトまたはグローバル | Node.jsライブラリ      |
| **Homebrew** | **システムソフトウェア全般** | **システム全体のみ**   | アプリケーション、ツール、言語本体 |
### Homebrewの役割

Homebrewは**macOS/Linux用のパッケージマネージャー**で、**システムレベルのソフトウェア**をインストールします。

Homebrewでインストールするもの

```
# プログラミング言語本体
brew install python          # Python自体
brew install node            # Node.js自体
brew install ruby            # Ruby自体

# コマンドラインツール
brew install git             # バージョン管理
brew install wget            # ファイルダウンロード
brew install ffmpeg          # 動画変換

# アプリケーション
brew install --cask chrome   # Chromeブラウザ
brew install --cask vscode   # VSCode
brew install --cask docker   # Docker
```


pip/npmでインストールするもの
```
# Pythonのライブラリ（Python内で使う）
pip install requests
pip install django

# JavaScriptのライブラリ（Node.js内で使う）
npm install express
npm install react
```

具体例で理解する

Webアプリを作る場合の流れ
```
# 1. Homebrewで言語本体をインストール（システムレベル）
brew install python
brew install node

# 2. プロジェクトを作成
mkdir my_webapp
cd my_webapp

# 3. Python用の仮想環境とパッケージ（プロジェクトレベル）
python -m venv venv
source venv/bin/activate
pip install flask

# 4. Node.js用のパッケージ（プロジェクトレベル）
npm install express
```

階層構造:
```
システムレベル（Homebrew）
  ├── Python本体
  ├── Node.js本体
  └── git, wgetなどのツール
      ↓
プロジェクトレベル（pip/npm）
  ├── Pythonライブラリ（flask, djangoなど）
  └── JavaScriptライブラリ（express, reactなど）
```

インストール先の違い

Homebrew
```
brew install python
# インストール先: /opt/homebrew/bin/python (M1/M2 Mac)
#              または /usr/local/bin/python (Intel Mac)
```
**影響範囲:** システム全体、全ユーザー

pip（仮想環境なし）
```
pip install requests
# インストール先: /opt/homebrew/lib/python3.x/site-packages/
```
**影響範囲:** システム全体のPythonスクリプト

pip（仮想環境あり）
```
source venv/bin/activate
pip install requests
# インストール先: ./venv/lib/python3.x/site-packages/
```
**影響範囲:** このプロジェクトのみ


### プロジェクトごとに分離できるか？

|ツール|プロジェクトごとの分離|
|---|---|
|**Homebrew**|❌ できない（システム全体に1つ）|
|**pip + 仮想環境**|✅ できる|
|**npm**|✅ できる（デフォルト）|

Homebrewの例:
```
# プロジェクトAでPython 3.10を使いたい
cd project_a
brew install python@3.10

# プロジェクトBでPython 3.12を使いたい
cd project_b
brew install python@3.12
# → システム全体が3.12に上書きされる（同時に使えない）
```

pipの例（仮想環境あり）:
```
# プロジェクトAでDjango 3.0
cd project_a
python -m venv venv
source venv/bin/activate
pip install django==3.0

# プロジェクトBでDjango 4.2
cd project_b
python -m venv venv
source venv/bin/activate
pip install django==4.2
# → 両方が共存できる
```
### 他のOSでの対応ツール

| OS            | システムパッケージマネージャー         |
| ------------- | ----------------------- |
| macOS         | **Homebrew**            |
| Ubuntu/Debian | `apt`                   |
| RedHat/CentOS | `yum` / `dnf`           |
| Windows       | `winget` / `chocolatey` |

これらは全て「システムレベル」のソフトウェアを管理します。
### まとめ: 階層的な理解
```
【システムレベル】Homebrew / apt / yum
  - Python本体、Node.js本体、gitなどをインストール
  - システム全体に影響
  - プロジェクトごとに分離できない
    ↓
【言語レベル】pip / npm / gem
  - ライブラリ・パッケージをインストール
  - プロジェクトごとに分離可能（pip=仮想環境、npm=デフォルト）
```

使い分け:
- Homebrew: 「Python自体」「Node.js自体」「エディタ」など、システム全体で使うものをインストール
- pip/npm: 「特定のプロジェクトで必要なライブラリ」をインストール
