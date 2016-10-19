---
layout: default
title: "GIT LIVE FLOW : GitHub FLOW + GIT FLOW"
meta:
    description: "Git live flowは、git flowとGitHub flowを合わせた、より良い開発フローです。"
    keywords: "Git live flow,git-live,git flow,GitHub flow,GitHub,git,開発フロー,ブランチモデル"
    author: "suzunone.eleven"
og:
    title: "GIT LIVE FLOW : GitHub FLOW + GIT FLOW"
    description: "Git live flowは、git flowとGitHub flowを合わせた、より良い開発フローです。"
    url: ""
    image: "img/og.jpg"
lang: ja

---
# Git Live Flow


## はじめに {#h-getting-started}
<span class="spell git-live-flow">Git Live Flow</span>は、<span class="spell php">PHP</span>で記述された、GitHub及びそれに類似するサービスを利用して行う、ブランチモデルです。


<span class="spell git-live">git-live</span>は<span class="spell git">Git</span>の拡張コマンドであり、<span class="spell git-live-flow">Git Live Flow</span>を行う為の高度なリポジトリ操作を提供します。


このサイトでは、<span class="spell git-live-flow">Git Live Flow</span>の流れと、<span class="spell git-live">git-live</span>の基本的な使い方を説明します。


## 入門 {#h-introduction}

<span class="spell git-live-flow">Git Live Flow</span>は<a href="http://nvie.com/posts/a-successful-git-branching-model/" rel="nofollow" target="_blank">git-flow</a>と同じく、マージを前提にした開発フローです。

<span class="spell git-live-flow">Git Live Flow</span>とgit-flow。この二つの考え方は非常によく似ています。

rebaseは行いません。リモートリポジトリを大量のコミットログで汚したくない場合は、squash mergeを行います。


## インストール・設定 {#h-installation}

 * <span class="spell git-live">git-live</span>は<span class="spell php">PHP</span>で記述された、<span class="spell git">Git</span>の拡張コマンド群です。あらかじめ、<span class="spell git">Git</span>と<span class="spell php">PHP</span>をインストールしておく必要があります。

 * <span class="spell git-live">git-live</span>は、、<span class="spell git">Git</span>と<span class="spell php">PHP</span>が動作する以下の環境で動作します。
     * MAC OS
     * Linux
     * Windows


### MAC OS

`````````````````````` shell
$ wget https://raw.GitHubusercontent.com/Git-Live/git-live/master/bin/git-live.phar -O git-live
``````````````````````

もしくは

`````````````````````` shell
$ curl https://raw.GitHubusercontent.com/Git-Live/git-live/master/bin/git-live.phar -o git-live
``````````````````````

その後、

`````````````````````` shell
$ chmod 0777 ./git-live
$ sudo mv ./git-live /usr/local/bin/git-live
``````````````````````

### Linux


`````````````````````` shell
$ wget https://raw.GitHubusercontent.com/Git-Live/git-live/master/bin/git-live.phar -O git-live
``````````````````````

もしくは

`````````````````````` shell
$ curl https://raw.GitHubusercontent.com/Git-Live/git-live/master/bin/git-live.phar -o git-live
``````````````````````

その後、

`````````````````````` shell
$ chmod 0777 ./git-live
$ sudo mv ./git-live /usr/local/bin/git-live
``````````````````````


### Windows
以下のファイルを、パスが通ったディレクトリにおいてください。

 * [https://raw.GitHubusercontent.com/Git-Live/git-live/master/git-live.php](https://raw.GitHubusercontent.com/Git-Live/git-live/master/git-live.php)
 * [https://raw.GitHubusercontent.com/Git-Live/git-live/master/bin/git-live.bat](https://raw.GitHubusercontent.com/Git-Live/git-live/master/bin/git-live.bat)


## <span class="spell git-live-flow">Git Live Flow</span>の要件 {#h-requirements}

 * リモートブランチは以下の三種類を用いる

<span class="spell repository_name">origin</span>
: Forkした自分専用のリモートリポジトリ。

<span class="spell repository_name">upstream</span>
: 公式リポジトリ（プロジェクトメンテナが作成した源流のリポジトリ）。
: write権限がない場合も設定する。
: write権限がある場合は、releaseおよびhotfixを行うことができる。

<span class="spell repository_name">deploy</span>
: リリース作業用のリポジトリ。別途用意する必要がない場合は、<span class="spell repository_name">upstream</span>と同一となる。
: 特にwebアプリケーションの開発現場では、リリース用のファイルをDMZに一度スタックさせる必要があることが多い。
: リリース用のリモートリポジトリを、GitHubの管理外にする事によって、リリース作業自体はGitHubの生死に依存しないと言うメリットもある。
: アクセス権限は、公式リポジトリと全く同じでないとならない。

 * 特別なブランチとして用意するのは以下の二つのみである

<span class="spell branch_name">develop</span>
: 開発用のブランチ。pull-requestの送信先は常にここになる。
: エンドユーザーではなく、開発者用の最新は常に<span class="spell branch_name">develop</span>ブランチとなり、ナイトリービルドを行う場合は、<span class="spell branch_name">develop</span>ブランチから行う

<span class="spell branch_name">master</span>
: 直接は操作されない。
: `git live release close`及び、`git live hotfix close`でのみ更新され、更新時には必ずタグが打ち込まれる。
: hotfix を行う際は、<span class="spell repository_name">upstream</span>リポジトリの<span class="spell branch_name">master</span>ブランチをベースとして行う。

 * リモートリポジトリはすべてベアリポジトリである必要がある
    * GitHubなどのサービスを利用していれば、リモートリポジトリはベアリポジトリとなります

 * release及び、hotfix可能なのは、公式リポジトリに変更権限を持つユーザーのみ

 * hotfix以外での源流へのマージは必ず、pull-request経由で行う


## <span class="spell git-live-flow">Git Live Flow</span>の手順

### 準備

#### プロジェクトメンテナは公式リポジトリを作成する

すべての源流となる、公式リポジトリを作成しプロジェクトメンバーに公開します。

リリースの権限を持つプロジェクトメンバーには、write権限を与える必要があります。

#### プロジェクトメンテナはデプロイリポジトリを作成する

公式リポジトリをForkしたデプロイ専用のリポジトリを用意することが出来ます。

必要が無い場合は、デプロイリポジトリと公式リポジトリを兼用することが出来ます。


特に、WEBアプリケーション開発の現場では、リリース用のファイルをDMZに一度スタックさせ、同一ネットワークにリリース用のファイルを置いた後、デプロイという流れを踏むことが多い。

デプロイリポジトリはそのためのリポジトリです。



#### 公式リポジトリのFork

この作業は、プロジェクトメンバーが開発を始める際一番初めに行う作業ですが、一度行えば二度と行う必要はありません。


#### リモートリポジトリをcloneする

開発用に、リモートリポジトリをcloneし、cloneしたリポジトリに正しいリモートリポジトリを定義する。

言葉で説明すると、とても難しそうですが、実際には、

`````````````````````` shell
$ git live init
``````````````````````

コマンドを実行し、幾つかの質問に答えるだけです。

``````````````````````
Please enter only your remote-repository.
: <fork したあなた専用のリポジトリ>
Please enter common remote-repository.
: <公式リポジトリ>
Please enter deploying dedicated remote-repository.
If you return in the blank, it becomes the default setting.
default:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
: <デプロイリポジトリ>（空で入力した場合は、公式リポジトリと同じ）
Please enter work directory path.
If you return in the blank, it becomes the default setting.
default:xxxxxxx
: <ディレクトリ名。空で入力した場合は、画面に表示されているディレクトリ名となる>
``````````````````````


この作業も、自分の開発環境を破棄しない限りは、二度と行う必要はありません



### 最も単純な流れ {#h-basic_flow}

#### <span class="spell branch_name">feature</span>ブランチの作成

すべての開発は、<span class="spell branch_name">feature</span>ブランチで行われます。

<span class="spell branch_name">feature</span>ブランチは、<span class="spell repository_name">upstream</span>の<span class="spell branch_name">develop</span>ブランチから作成されます。

`````````````````````` shell
$ git live feature start <featureの名前>
``````````````````````

#### <span class="spell branch_name">feature</span>のpush

<span class="spell branch_name">feature</span>ブランチでの開発が終わったら、<span class="spell repository_name">origin</span>リポジトリにpushします。

`````````````````````` shell
$ git live push
``````````````````````

もしくは

`````````````````````` shell
$ git live feature push
``````````````````````

で行えます。

どちらも、現在選択されている<span class="spell branch_name">feature</span>ブランチをpushしますが、以下のような違いがあります。

 * `git live push`
     * 作業用リポジトリを最新化せずにpushする
 * `git live feature push`
     * 作業用リポジトリを最新化してpushする

#### プルリクエストを出し、コードレビューを受ける/行う


<span class="spell repository_name">origin</span>にpushした、<span class="spell branch_name">feature</span>ブランチから、<span class="spell repository_name">upstream</span>の<span class="spell branch_name">develop</span>ブランチへpull-request
を出します。

コードレビューを行い、問題がなければマージします。


レビュー者は以下のコマンドを利用して、出されたpull-requestの内容を、自分のローカルリポジトリに取得することが出来ます。

`````````````````````` shell
$ git live pr track <pull-requestのissue番号>
``````````````````````

`git live pr track`したコードを最新のpull-requestに更新する場合は、

`````````````````````` shell
$ git live pr pull
``````````````````````

で行います。

`git live pr pull`は、`git live pr track`で自動作成されたブランチで行う必要があります。


#### リリース作業を行う


リリース作業を行うために、<span class="spell branch_name">release</span>ブランチを作成します。

<span class="spell branch_name">release</span>ブランチは、<span class="spell repository_name">upstream</span>の<span class="spell branch_name">develp</span>ブランチから作成します。


この<span class="spell branch_name">release</span>ブランチブランチが作成された状態を、<span class="spell git-live-flow">Git Live Flow</span>では、
<span class="spell release-open">release open</span>状態と定義してます。

<span class="spell release-open">release open</span>するには、以下のコマンドを実行します。

`````````````````````` shell
$ git live release open
``````````````````````

<span class="spell branch_name">release</span>ブランチは、リリース作業が完了次第削除します。

つまり、<span class="spell branch_name">release</span>ブランチは常に一つであり、
<span class="spell branch_name">release</span>ブランチが作成済の場合は、先に前回のリリースを終わらせる(release closeする)必要があります。

すでにrelease openされている場合、`git live release open`は失敗します。

`git live release open`可能かどうかを調べるには以下のコマンドを実行します。


`````````````````````` shell
$ git live release state
``````````````````````

結果の最終行が、、

``````````````````````
release is close.
``````````````````````

となった場合、<span class="spell release-open">release open</span>可能です。

最終的なビルドの作成、あるいは、ソースコードのデプロイは、<span class="spell release-open">release open</span>した状態で行います。


<span class="spell release-open">release open</span>した状態で、不具合が見つかりコードを修正したくなった場合は、以下の方法で修正可能です。

1. コードを修正し、<span class="spell repository_name">upstream</span>に作成されたreleaseブランチにプルリクエストを出します
2. 修正が問題ないことを確認したら、コードをマージします
3. 以下のコマンドを実行します

`````````````````````` shell
$ git live release sync
``````````````````````

もしくは、ローカルに作成されたreleaseブランチのコードを直接修正し、commitした後、以下のコマンドを実行します。


`````````````````````` shell
$ git live release sync
``````````````````````

誰かが、<span class="spell release-open">release open</span>したreleaseブランチを、手元に取得する場合は、以下のコマンドを実行します。

また、誰かの変更を取り込む場合も同様です。

`````````````````````` shell
$ git live release sync
``````````````````````

<span class="spell release-open">release open</span>した状態で、

`````````````````````` shell
$ git live release state
``````````````````````

コマンドを実行すると、今回リリースされる予定のコードの一覧を見ることが出来ます。


また、<span class="spell release-open">release open</span>した状態で、<span class="spell branch_name">develop</span>ブランチにマージを行うことは出来ません。

正確には、<span class="spell release-open">release open</span>した状態で行われた、<span class="spell branch_name">develop</span>ブランチへの変更は、すべて<span class="spell branch_name">release</span>ブランチに取り込む必要があります。

#### リリースを完了し<span class="spell release-close">release close</span>する

すべてのリリース作業が完了したら、<span class="spell branch_name">release</span>ブランチを<span class="spell branch_name">master</span>ブランチと<span class="spell branch_name">develop</span>ブランチにマージし、
<span class="spell branch_name">release</span>ブランチを削除し、タグを打ちます。

これらを、<span class="spell git-live-flow">Git Live Flow</span>では<span class="spell release-close">release close</span>といいます。


実際には、

`````````````````````` shell
$ git live release close
``````````````````````

コマンドで自動的に行うことが出来ます。

タグも、自動的に打たれます。

別途GitHub上でリリースノートを書く必要がある場合は、GitHub上で作業してください。


<span class="spell branch_name">develop</span>ブランチが、<span class="spell branch_name">release</span>ブランチよりも進んでいる場合、<span class="spell release-close">release close</span>は失敗します。

どうしても、<span class="spell branch_name">develop</span>ブランチの更新内容を、<span class="spell branch_name">release</span>ブランチに取り込みたくない場合は、

`````````````````````` shell
$ git live release close-force
``````````````````````

を試すことが出来ます。

これは、developブランチへの変更を無視して、<span class="spell release-close">release close</span>する事ができます。

ただし、自動的に行われるmergeでConflictしないように、<span class="spell branch_name">release</span>ブランチの内容をすべて手動で、<span class="spell branch_name">develop</span>ブランチにマージしておいてください。


### ホットフィックスの流れ {#h-hotfix_flow}


#### <span class="spell hotfix-open">hotfix open</span>する

実際に運用を続けていると、<span class="spell branch_name">develop</span>ブランチに、幾つかの<span class="spell branch_name">feature</span>をマージ済だが、
<span class="spell branch_name">develop</span>ブランチへの変更を無視して、緊急性の高いコードだけを個別にリリースしたいと言うようなことがあります。

このホットフィックスを実現するためには、まず、<span class="spell branch_name">master</span>ブランチから<span class="spell branch_name">hotfix</span>ブランチを作成する必要があります。

この<span class="spell branch_name">hotfix</span>ブランチが作成されている状態を、<span class="spell git-live-flow">Git Live Flow</span>では、
<span class="spell hotfix-open">hotfix open</span>状態と定義してます。

リリースと同じく、ホットフィックスも、多重に<span class="spell hotfix-open">hotfix open</span>することは出来ません。

<span class="spell hotfix-open">hotfix open</span>するには、前回のhotfixを<span class="spell hotfix-close">hotfix close</span>する必要があります。

<span class="spell hotfix-open">hotfix open</span>可能か調べるには、以下のコマンドを実行します。


`````````````````````` shell
$ git live hotfix state
``````````````````````

<span class="spell hotfix-open">hotfix open</span>された状態で、`git live hotfix state`コマンドを実行すると、更新されるファイルの一覧を取得することが出来ます。


#### コードを修正する

ホットフィックスでのコードの修正は、<span class="spell release-open">release open</span>状態でのコードの修正とよく似ています。


プルリクエストの宛先が<span class="spell branch_name">release</span>ブランチではなく、<span class="spell branch_name">hotfix</span>ブランチとなることと、実行するコマンドが`git live release sync`ではなく、以下のように変更になることだけです。


`````````````````````` shell
$ git live hotfix sync
``````````````````````

#### ホットフィックスを完了し<span class="spell hotfix-close">hotfix close</span>する
すべてのホットフィックス作業が完了したら、<span class="spell branch_name">hotfix</span>ブランチを<span class="spell branch_name">master</span>ブランチと<span class="spell branch_name">develop</span>ブランチにマージし、
<span class="spell branch_name">hotfix</span>ブランチを削除し、タグを打ちます。

これらを、<span class="spell git-live-flow">Git Live Flow</span>では<span class="spell hotfix-close">hotfix close</span>といいます。


実際には、

`````````````````````` shell
$ git live release close
``````````````````````

コマンドで自動的に行うことが出来ます。

タグも、自動的に打たれます。

別途GitHub上でリリースノートを書く必要がある場合は、GitHub上で作業してください。





## コマンドリファレンス {#h-command_reference}


