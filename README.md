# GitHubCLITest_202203

> - [Download Page](https://cli.github.com/)
> - [Manual Page](https://cli.github.com/manual/gh)

## Command List

|**Core commands**|descripition|Sub commands|
|:-|:-|:-|
|gh **auth**|GitHubとのログイン認証|login logout refresh setup-git status|
|gh **browse**|画面表示|-|
|gh **codespace**|クラウド仮想環境の操作|code cp create delete edit list logs ports forward visibility ssh stop|
|gh **gist**|Gistの操作|clone create delete edit list view|
|gh **issue**|イシューの操作|close comment create delete edit list reopen status transfer view|
|gh **pr**|プルリクの操作|checkout checks close comment create diff edit list merge ready reopen review status view|
|gh **release**|リリースの操作|create delete-asset delete download list upload view|
|gh **repo**|リポジトリの操作| archive clone create delete deploy-key add delete list edit fork list rename sync view|

|**Actions commands**|description|Sub commands|
|:-|:-|:-|
|gh **run**|コンテナビルド操作|cancel download list rerun view watch|
|gh **workflow**|作業の流れの操作|disable enable list run view|

|**Additional commands**|description|Sub commands|
|:-|:-|:-|
|gh **alias**|ショートカットの操作|delete list set|
|gh **api**|外部ライブラリ|-|
|gh **completion**|コード補完|-|
|gh **config**|設定ファイル|get list set|
|gh **extension**|拡張機能の操作|create install list remove upgrade|
|gh **gpg-key**|公開鍵の生成と一覧|add list|
|gh **help**|ヘルプ|environment formatting mintty reference|
|gh **search**|検索操作|repos|
|gh **secret**|一覧と削除|list remove set|
|gh **ssh-key**|秘密鍵の生成と一覧|add list|

## GitHubとのログイン認証

```bash
$ gh auth login

? What account do you want to log into? GitHub.com
? What is your preferred protocol for Git operations? SSH
? Upload your SSH public key to your GitHub account? /Users/arihito/.ssh/github_rsa.pub
? How would you like to authenticate GitHub CLI? Login with a web browser

# 以下の8桁のトークンをコピーして、
# ブラウザにコードを貼り付けるとアカウント認証が完了
! First copy your one-time code: XXXX-XXXX 
Press Enter to open github.com in your browser... 
✓ Authentication complete.
- gh config set -h github.com git_protocol ssh
✓ Configured git protocol
✓ Uploaded the SSH key to your GitHub account: /Users/userName/.ssh/github_rsa.pub
✓ Logged in as userName
```
## 基本操作

```bash
# リモートリポジトリを作成と同時にクローン
$ gh repo cerate --public --clone [projectName]

# 現在いるプロジェクト内からそのプロジェクト名を使用し自動でリモートリポジトリを作成
$ gh repo create

# リモートリポジトリのREADMEをコンソールに表示(リポジトリ指定も可能)
$ gh repo view

# プルリクの作成
$ gh pr create

# プルリクのマージ
$ gh pr merge

# プルリクのレビュー
$ gh pr review

# プルリクの内容を閲覧
$ gh pr view

# イシューの作成
$ gh issue create

# イシューの閉鎖
$ gh issue close

# イシューの一覧表示
$ gh issue list

# イシューの再オープン
$ gh issue list

# 自身PR一覧を取得
$ gh pr list -s all -a [自分のアカウント名>] | grep "検索文字列"
```
