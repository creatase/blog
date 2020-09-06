---
title: Javaでドラクエっぽい対戦ゲームを作ってみる【お題１＋寄り道編】
date: 2018-04-20T09:52:53
description: 前回に引き続き、Javaでドラクエっぽい対戦ゲームを作っていきたいと思います。今回からいよいよ実際にお
slug: java_nimigame_2
category: Java
tags: 
keywords: 
---

こんにちは、ゆきたです。

前回に引き続き、Javaでドラクエっぽい対戦ゲームを作っていきたいと思います。今回からいよいよ実際にお題をコードにしていきます。

参考：まゆさんの記事はコチラ→[【体験談】神里よしとさんのレッスンで、 Ruby完全初心者が対戦ゲームを作ってみた](https://www.mayuowl.com/ruby-first/)

&nbsp;

## &nbsp;お題１　ゆきた、スライムのHPを宣言

&nbsp;

### クラスを宣言する

まずはじめに、MiniGameクラスとmainメソッドを宣言します。

public class MiniGame { public static void main(String[] args) { } }

Javaでは、実行するファイル名とその中でmainメソッドを持つクラス名が一致していなければなりません。なのでクラス名はMiniGameクラスとします。Atomには、入力の補完機能がついているので、mainとだけ打ってreturnキーを押せば、自動的にmainメソッドの構文が補完されます。

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-8.06.28.png)

この状態でreturnキーを押すと↓

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-8.06.58.png)

ボコンと自動で書いてくれます。便利機能はどんどん使います。

&nbsp;

### HPを宣言して表示してみる

次に、ゆきたとスライムそれぞれのHPを宣言して、表示させてみます。とりあえず何も考えずstatic intとしてそれぞれ宣言します。宣言したHPの変数を使って、mainメソッド内にHPを表示させるコードを書きます。（ちなみにplと入力→returnでSystem.out.println{}が自動補完されて書かれます。便利機能はどんどん使います）

public class MiniGame { static int yukitaHp = 100; static int slimeHp = 100; public static void main(String[] args) { System.out.println("--ゆきたHP：" + yukitaHp + " --"); System.out.println("--スライムHP：" + slimeHp + " --"); } } 

&nbsp;

ここで一度コンパイルして実行してみます。

Javaでは今書いているようなソースコードをコンパイルして一旦バイトコードに変換します。そして変換後のバイトコードを実行環境で実行して、プログラムを動かすという手順を踏みます。コンパイルでは平たく言えばプログラムの書き方が合っているかどうかがチェックされます。なので、ルール通りに書けていないとコンパイルエラーとなってエラー内容が表示されます。何事もなくコンパイルが通れば何も表示されません。

ではやってみます。コンパイルはjavacコマンドを使います。

$ javac MiniGame.java

するとこんな表示が、

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-8.53.34.png)

おぉ…。

これはコンパイルエラーではないのですが、いわゆる文字化けが起こっています。macターミナルが標準でSJISが指定されているためのようで、ソースコードに合わせてUTF-8を指定する必要があります。ちなみにコンパイル自体は通っていて、.classファイルも生成されています。ただし実行しても下のような状態。

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-8.59.53.png)

やはり文字コードを指定し直す必要があります。文字コードの指定は、ホームディレクトリに.bash\_profileを作成することで実現します。

参考記事→[MacのターミナルやiTermでのJavaエラーメッセージ文字化け解消](https://qiita.com/fgnhssb/items/519a614b42e24330fbc4)

ターミナルでホームディレクトリに移動して

$ cd ~

.bash\_profileを作成してvim（ターミナル上で動くエディタ）で内容を編集します。

$ vi .bash\_profile

実行するとvimが立ち上がり下のような編集画面に変わります。

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-9.20.22.png)

vimの操作は少し慣れが必要ですが、落ち着いて操作すれば問題ありません。まず i キーを押してインサートモードに切り替えます。

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-9.24.17.png)

左下の表示が– INSERT –に変わればインサートモードになっています。この状態で以下の内容をコピペします。

alias javac='javac -J-Dfile.encoding=UTF-8' alias java='java -Dfile.encoding=UTF-8'

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-9.27.32.png)

この状態にできたら、コマンドモードに戻ります（最初の状態）。コマンドモードに戻るにはescキーを押します。

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-9.30.14.png)

左下の– INSERT –の表示が消えればコマンドモードになっています。この状態で変更したこの.bash\_profileファイルを保存してvimを終了します。保存するコマンドは :w、vimを終了するコマンドは :qですが、これらをまとめて :wqとしても実行できるので今回はこのコマンドを使います。

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-9.34.15.png)

入力したコマンドは左下に表示されています。コマンドが正しいことを確認してreturnキーを押すと、コマンドが実行されてvimが閉じターミナルの表示に戻ります。

それでは気を取り直してコンパイルしてみます。

$ javac MiniGame.java

今度はうまくいきました。（何も表示されなくて成功です）

さらに実行してみます。実行はjavaコマンドを使います。指定するファイル名は.classを除いたもので指定します。

$ java MiniGame

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-9.46.25.png)

今度はうまく表示されましたね。

&nbsp;

次回はお題２以降をやってみます。（お題１しかできなかった…）

&nbsp;

- [Javaでドラクエっぽい対戦ゲームを作ってみる【準備編】](https://creatase.info/java_minigame_1/)
- Javaでドラクエっぽい対戦ゲームを作ってみる【お題１+寄り道編】←いまココ
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題２編】](https://creatase.info/java_nimigame_3/)
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題３-４編】](https://creatase.info/java_minigame_4/)
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題５-7編】](https://creatase.info/java_minigame_5/)
