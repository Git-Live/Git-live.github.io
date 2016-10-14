---
layout: default
title: "GIT LIVE FLOW : GIT HUB FLOW + GIT FLOW"
meta:
    description: "Git live flowは、git flowとgithub flowを合わせた、より良い開発フローです。"
    keywords: "Git live flow,git-live,git flow,github flow,github,git,開発フロー,ブランチモデル"
og:
    title: "GIT LIVE FLOW : GIT HUB FLOW + GIT FLOW"
    description: "Git live flowは、git flowとgithub flowを合わせた、より良い開発フローです。"
    url: ""
    image: "img/og.jpg"
---

## はじめに {#h-getting-started}
<span class="git-live-flow">git-live-flow</span>は、<strong>PHP</strong>で記述された、git hub及びそれに類似するサービスを利用して行う、ブランチモデルです。


<span class="git-live">git-live</span>は<strong>git</strong>の拡張コマンドであり、<span class="git-live-flow">git-live-flow</span>を行う為の高度なリポジトリ操作を提供します。


このサイトでは、<span class="git-live-flow">git-live-flow</span>の流れと、<span class="git-live">git-live</span>の基本的な使い方を説明します。


## 入門 {#h-introduction}

 * <span class="git-live-flow">git-live-flow</span>は。git-flowと同じく、マージを前提にした開発フローです。rebaseは行いません。リモートリポジトリを汚したくない場合は、squash mergeを行います。

## インストール・設定 {#h-installation}

 * <span class="git-live">git-live</span>は<strong>PHP</strong>で記述された、<strong>git</strong>の拡張コマンド群です。あらかじめ、<strong>git</strong>と<strong>PHP</strong>をインストールしておく必要があります。

 * <span class="git-live">git-live</span>は、、<strong>git</strong>と<strong>PHP</strong>が動作する以下のJAN強で動作します。
     * MAC OS
     * Linux
     * Windows


### MAC OS

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ wget https://raw.githubusercontent.com/Git-Live/git-live/master/bin/git-live.phar -O git-live
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

もしくは

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ curl https://raw.githubusercontent.com/Git-Live/git-live/master/bin/git-live.phar -o git-live
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

その後、

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ chmod 0777 ./git-live
$ sudo mv ./git-live /usr/local/bin/git-live
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Linux


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ wget https://raw.githubusercontent.com/Git-Live/git-live/master/bin/git-live.phar -O git-live
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

もしくは

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ curl https://raw.githubusercontent.com/Git-Live/git-live/master/bin/git-live.phar -o git-live
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

その後、

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
$ chmod 0777 ./git-live
$ sudo mv ./git-live /usr/local/bin/git-live
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


### Windows
以下のファイルを、パスが通ったディレクトリにおいてください。

 * https://raw.githubusercontent.com/Git-Live/git-live/master/git-live.php
 * https://raw.githubusercontent.com/Git-Live/git-live/master/bin/git-live.bat


## <span class="git-live-flow">git-live-flow</span>の要件
 * リモートブランチは以下の三種類を用いる

origin
: Forkした自分のアカウント管理配下にあるリポジトリ

upstream
: 源流のリポジトリ

release
: リリース作業用のリポジトリ。別途用意する必要がない場合は、upstreamと同一となる。
: 特にwebアプリケーションの開発現場では、リリース用のファイルをDMZに一度スタックさせる必要があることが多い。
: リリース用のリモートリポジトリを、Git Hubの管理外にする事によって、リリース作業自体はGit Hubの生死に依存しないと言うメリットもある。

 * 特別なリポジトリとして用意するのは以下の二つのみである

develop
: 開発用のリポジトリ。pull-requestの送信先は常にここになる。
: エンドユーザーではなく、開発者用の最新は常にdevelopブランチとなり、ナイトリービルドを行う場合は、developから行う

master
: エンドユーザー向けのリポジトリであり、直接は操作されない。
: `git live release close`及び、`git live hotfix close`でのみ更新され、更新時には必ずタグが打ち込まれる



 * hotfix（超緊急対応）以外での源流へのマージは必ず、pull-request経由で行う
 *



## 開発を始める前に

<span class="git-live-flow">git-live-flow</span>を始める前に、幾つかの下準備が必要です。



### ProjectのFork

<span class="git-live-flow">git-live-flow</span>では、最終的にpull-requestによって源流のリポジトリにmergeを行う為、Forkを行います。





