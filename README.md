# GitHubCLITest_202203

## インストール

Macは[Homebrew](https://brew.sh/index_ja)のパッケージマネージャを入れておくか[インスートラー](https://cli.github.com/)を使用する。

Windowsは事前に[Winget](https://reffect.co.jp/html/winget)か[scoop](https://nodachisoft.com/common/jp/article/jp000013/)などのパッケージマネージャをインストールしておく。

```bash
# Mac
$ brew install gh


# win winget
$ winget install --id GitHub.cli

# win scoop
$ scoop install gh
```

## ghコマンドの登録

`gh`コマンドが使用できるように環境変数のパスを通す。

```bash
# .zshrc for Mac
$ zshrc
# 以下の1行を追記
eval "$(gh completion -s zsh)"
$ rezshrc

# .bashrc for Windows
$ bashrc
# 以下の1行を追記
eval "$(gh completion -s bash)"
$ rebashrc
```

## GitHubとのログイン認証

ghコマンドを使用してGitHubとの認証を行う。

SSHキーの設定が必要になるため、元からある自身のいずれかのキー(.pubが付いた公開鍵)を選択すれば、自動でGitHubに登録される。

また、コンソール上に表示された8桁のトークンコードをハイフンを含めてコピーし、「**Enter**」キーを押す。すると、自動でブラウザが起動し、トークンの入力フォームが表示されるため、そのまま貼り付けると認証が行われる。

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

## コマンド一覧

基本は`gh`の後にコアコマンド、その後に必要があればリポジトリ名やオプションとなるFlagの順番で追記していく。

詳細は[Manual Page](https://cli.github.com/manual/gh)を参照する。

```bash
$ gh corecommand [repository] [flag]
```

|**Core commands**|descripition|Sub commands|
|:-|:-|:-|
|gh **auth**|GitHubとのログイン認証|login logout refresh setup-git status|
|gh **browse**|画面表示|-|
|gh **codespace**|ブラウザ上の疑似VSCode|code cp create delete edit list logs ports forward visibility ssh stop|
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


## 基本操作

```bash
# リモートリポジトリの作成
# ghcr repoName
$ gh repo create [repoName]

# リモートリポジトリを作成し、同階層にクローンを行う
$ gh repo create --public --clone [projectName]

# 引数の名前でリポジトリを作成、メインブランチに変換後READMEを作成しアドコミット後プッシュ
# ghcl repoName
alias ghcl='(){gh repo create $1 --public --clone && cd $1 && echo "# $1" > README.md && gacm "first commit" && gmain && gpom && ls -la && cat README.md}'

# 同期中のリモートリポジトリのREADMEをコンソール上に表示(リポジトリ指定も可能)
# ghvw [repoName]
$ gh repo view [repoName]

# 同期中のリモートリポジトリを削除(一定時間経過後はトークンの再認証が必要)
$ gh repo delete --confirm

# 同期中のリモートリポジトリを削除し、今いるプロジェクトディレクトリを抜けてから削除
# ghdl
alias ghdl='(){gh repo delete --confirm && var="${PWD##*/}" && cd ../ && rm -rf $var} && ls'


# イシューの作成
# ghisc
$ gh issue create
## -t　タイトルの記述
## -b　本文の記述
## -a　参加者(@ID)の追加
## -l　ラベル名の追加
## -p　カンバン方式名の追加
## -m　マイルストーン名の追加
## -w　ブラウザで入力フォームを開く

# イシューの閉鎖(再入力が必要)
# ghisd [issueNo]
$ gh issue delete [issueNo]

# イシューの一覧表示
# ghisl
$ gh issue list


# プルリクの作成
# ghprc
$ gh pr create
## リポジトリ(デフォルトブランチ)選択/スキップ/キャンセル
## プルリクタイトル
## プルリク本文
## 提出/ブラウザで続ける/その他の情報追加/キャンセル

# プルリクのレビュー
# ghprr
$ gh pr review
# - コメント/承認許可/プルリク変更
# - コメント：eでvimを起動し編集後に自動コミット

# プルリクの内容を閲覧
# ghprv
$ gh pr view
# qで閲覧終了

# プルリクのマージ
# ghprm
$ gh pr merge
# マージ(通常)/リベース(根本移動)/スカッシュ(退避)
# プルリクしたブランチの削除(初期値はNo)
# 実行/タイトル編集/本文編集/キャンセル
```

## その他のコマンド例

APIを使用してGraphQLのqueryを実行し自分のリポジトリ一覧を取得

```bash
$ gh api graphql --paginate -f query='
  query($endCursor: String) {
    viewer {
      repositories(first: 100, after: $endCursor) {
        nodes { nameWithOwner }
        pageInfo {
          hasNextPage
          endCursor
        }
      }
    }
  }
'
```

現在のリポジトリ内でPR一覧からキーワード検索

```bash
$ gh pr list -s all -a [自分のアカウント名] | grep "検索文字列"
```

myprとしてエイリアスに登録してからキーワード検索

```bash
$ gh alias set mypr "pr list -s all -a [自分のアカウント名]"
$ gh mypr | grep "検索文字列"
```
