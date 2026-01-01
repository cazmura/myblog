+++
title = "pip install pytest を叩いてみたら"
date = "2026-01-01T13:05:03+09:00"
draft = false
tags = ["エラーメッセージ"]
images = []
+++

「よし。pytest 書いてみるぞ。」と思って pip install pytest を叩いてみたらエラーが返ってきた。

```
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try brew install
    xyz, where xyz is the package you are trying to
    install.
    
    If you wish to install a Python library that isn't in Homebrew,
    use a virtual environment:
    
    python3 -m venv path/to/venv
    source path/to/venv/bin/activate
    python3 -m pip install xyz
    
    If you wish to install a Python application that isn't in Homebrew,
    it may be easiest to use 'pipx install xyz', which will manage a
    virtual environment for you. You can install pipx with
    
    brew install pipx
    
    You may restore the old behavior of pip by passing
    the '--break-system-packages' flag to pip, or by adding
    'break-system-packages = true' to your pip.conf file. The latter
    will permanently disable this error.
    
    If you disable this error, we STRONGLY recommend that you additionally
    pass the '--user' flag to pip, or set 'user = true' in your pip.conf
    file. Failure to do this can result in a broken Homebrew installation.
    
    Read more about this behavior here: <https://peps.python.org/pep-0668/>

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
```

**日本語訳**
```
エラー: 外部管理環境

× この環境は外部で管理されています
╰─> Python パッケージをシステム全体にインストールするには、brew install
xyz を試してください。xyz はインストールしようとしているパッケージです。

Homebrew に含まれていない Python ライブラリをインストールする場合は、
仮想環境を使用してください。

python3 -m venv path/to/venv
source path/to/venv/bin/activate
python3 -m pip install xyz

Homebrew に含まれていない Python アプリケーションをインストールする場合は、
「pipx install xyz」を使用するのが最も簡単です。このコマンドは仮想環境を自動的に管理します。
pipx は

brew install pipx でインストールできます。

pip に '--break-system-packages' フラグを渡すか、pip.conf ファイルに 'break-system-packages = true' を追加することで、pip の以前の動作に戻すことができます。後者は
このエラーを永続的に無効にします。

このエラーを無効にする場合は、pip に '--user' フラグを追加するか、pip.conf ファイルで 'user = true' を設定することを強くお勧めします。
これを行わないと、Homebrew のインストールが壊れる可能性があります。

この動作の詳細については、<https://peps.python.org/pep-0668/> をご覧ください。

注: これが誤りだと思われる場合は、Python インストールまたは OS ディストリビューションのプロバイダにお問い合わせください。--break-system-packages を渡すことで、この設定を上書きできますが、Python インストールまたは OS が壊れる可能性があります。
ヒント: 詳細な仕様については、PEP 668 を参照してください。
```


### 何が起こっている？

- 「外部の環境」とは Homebrew のこと
- 私のシステムのPythonはHomebrewによってインストール・管理されている
- Homebrewが依存関係やパッケージのバージョンを適切に保つために、直接`pip install`でシステム全体にパッケージをインストールすることを制限している

### なぜ、直接 pip install することが制限されているのか

`pip`で直接システム全体にパッケージをインストールすると以下の問題が起こるため
- Homebrewが管理しているパッケージと競合する可能性がある
- システムが壊れるリスクがある
- 依存関係の問題が発生しやすい

### どうやって解消する？

エラーメッセージが提案している通り、以下の方法がある

1. 仮想環境を使う(最も推奨):
```
python3 -m venv myenv
source myenv/bin/activate
pip install パッケージ名
```


2. Homebrewでインストールできる場合:
```
brew install パッケージ名
```

3. pipxを使う(アプリケーションの場合):
```
brew install pipx
pipx install パッケージ名
```

仮想環境を使えば、プロジェクトごとに独立したPython環境を作れるので、システムに影響を与えずに安全にパッケージをインストールできる。

### 参考記事
- https://qiita.com/toiee_kame/items/c3781abb53f385dae4f9
- https://zenn.dev/kail/articles/ef502d950e8268