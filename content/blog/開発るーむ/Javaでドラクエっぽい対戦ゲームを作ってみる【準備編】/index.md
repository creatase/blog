---
title: Javaでドラクエっぽい対戦ゲームを作ってみる【準備編】
date: 2018-04-19T21:03:57
description: 今回は先日読んだ、私と同じくエンジニアを目指されているまゆさんのブログ記事の内容が面白かったので、その
slug: java_minigame_1
category: Java
tags: 
keywords: 
---

こんにちは、ゆきたです。

今回は先日読んだ、私と同じくエンジニアを目指されているまゆさんのブログ記事の内容が面白かったので、その記事を参考にドラクエっぽい対戦ゲームを作ってみます。記事の中ではRubyを使われていますが、せっかくなので私はJavaで作ってみたいと思います。

まゆさんの記事はコチラ→[【体験談】神里よしとさんのレッスンで、 Ruby完全初心者が対戦ゲームを作ってみた](https://www.mayuowl.com/ruby-first/)

## 環境・使用ツール

- macOS 10.13.4
- ターミナル
- [Atom](https://atom.io/) / Eclipse

ターミナルはmacに標準で付いていますね。Atomは無料のエディタで[ドットインストール](https://dotinstall.com/)で紹介されていたのがきっかけで私は使っています。書きやすい。もちろん他のエディタでも構いません。Eclipseをインストールしている人はそちらの方が書き間違いや書き忘れが少ないかもしれない。

![](スクリーンショット-2018-04-19-15.51.26-1.png)

作業画面はこんな感じ（左がAtomで右がターミナル）

## 下準備

まず最初にターミナルでJavaのバージョンを確認しておきます。
```bash
$ javac -version
```
上のコマンドでコンパイラのバージョンが表示されます（バージョンの番号はこの数字以上なら違っていても多分大丈夫）。
```bash
javac 1.6.0\_65
```
続いて実行環境のバージョンを確認。
```bash
$ java -version
```
下のようにバージョンの情報が表示されれば大丈夫。
```bash
java version "1.6.0_65"
Java(TM) SE Runtime Environment (build 1.6.0_65-b14-468)
Java HotSpot(TM) 64-Bit Server VM (build 20.65-b04-468, mixed mode)
```
次に、コードを書くjavaファイルを作ります。今回はホームディレクトリの下に「myGames」というフォルダを作って、その中にMiniGame.javaを作成して、このファイルにコードを書いて保存していきます。
```bash
$ cd ~
$ mkdir myGames
$ cd !$
$ touch MiniGame.java
```
!$はコマンドに渡した最後の文字列を呼び出します。この場合は「myGames」となり、コマンドとしてはcd myGamesが実行されることになります。

早速、Atomで「MiniGame.java」を開いてみます。

![](スクリーンショット-2018-04-19-17.02.43.png)

こんな風に表示されます。（初めて使う人は背景が白いかもしれませんが問題ないです）

空っぽ。touchコマンドでは中身が空のファイルが作られます。

それでは、今回作るゲームのお題を確認してみましょう。

1. ゆきた、スライムのHPを宣言
2. 「戦う」か「逃げる」かを選択できるようにする
3. まず、ゆきたが攻撃。４回に１回「会心の一撃」を繰り出せる
4. 攻撃を受けたスライムの様子を出力する（ダメージによって反応が変わる）
5. ゆきたの攻撃後、スライムが攻撃してくる
6. ゆきたの受けたダメージ数と、残りのHP数を出力する
7. どちらかが死ぬまで、攻撃をループさせる

おぉ…多い。

次回から一つ一つお題をコードにしてみたいと思います。

- Javaでドラクエっぽい対戦ゲームを作ってみる【準備編】←いまココ
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題１+寄り道編】](https://creatase.info/java_nimigame_2/)
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題２編】](https://creatase.info/java_nimigame_3/)
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題３-４編】](https://creatase.info/java_minigame_4/)
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題５-7編】](https://creatase.info/java_minigame_5/)
