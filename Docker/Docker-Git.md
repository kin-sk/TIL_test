# 【重要】初回のみ必要な設定：コンテナ内のGitに名前とEmailを教える

ローカルのGitに設定したように、コンテナ内のGitにも「誰がコミットしたか」を教える必要があります。これはコンテナごとに初回の一度だけ行います。

### 手順

バックエンドまたはフロントエンド、どちらでも良いので、コンテナに接続しているVSCodeのウィンドウを開きます。
VSCodeでターミナルを開きます (Ctrl+Shift+@ またはメニューから)。
そのターミナルで、以下のコマンドを実行してください。
（ご自身のGitHubの名前とメールアドレスに置き換えてください）

Generated bash```
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```
