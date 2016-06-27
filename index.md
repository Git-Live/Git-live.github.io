---
layout: top
nav-language_select: 言語選択
lang: Ja
outline-header: Git Live
outline-lead: git-liveはGit Live Flowで開発を行う為のコマンド群です。
outline-download-from-github: GitHubからダウンロード
outline-download-translate-this-manual: このマニュアルを翻訳する
outline-download-visit-git-flow: git-flowについて
outline-download-visit-github-flow: github-flowについて
title: TOPページ
---

# Project Git Live!


## 初めに

__git-live__はgitの拡張コマンドであり、Vincent Driessenの提唱するブランチモデルである、git-flowをgit hubと連携させてより良いブランチモデルを実現させるための、リポジトリ操作を提供します。


git-liveでは、git-flowをgit-hubを交えて運用するために、git-flowブランチモデルを拡張しています。

便宜的に、__Git Live flow__と呼びます。

このページでは、__git-live__の基本的な使い方と、__Git Live flow__の運用方法を説明しています。

## 注意

 * __Git Live flow__では、git-flowと同じくmergeする事をベースとして考えます。rebaseは行いません。
 * git-flowはすべてコマンドライン上で解決する、クールなソリューションですが、__Git Live flow__では、git hubが提供するGUIも使用します。
 * __git-live__は、コマンドラインの集合です。実行されているコマンドとその出力はすべて出力されますので、何が起こっているのかを確認して下さい。


## インストール

 * git と php(5.3以上)をインストールしておく必要があります。
 * __git-live__は、OSX、Linux、Windowsで動作します。

