---
title: Javaでドラクエっぽい対戦ゲームを作ってみる【お題２編】
date: 2018-04-20T14:03:49
description: ただいまJavaでドラクエっぽい対戦ゲームを作っている途中です。前回やっとの事でプレーヤーとスライムの
slug: java_nimigame_3
category: Java
tags: 
keywords: 
---

こんにちは、ゆきたです。

ただいまJavaでドラクエっぽい対戦ゲームを作っている途中です。前回やっとの事でプレーヤーとスライムのHPを表示させるお題をクリア。今回はお題２以降をコードにしていってみたいと思います。

参考：まゆさんの記事はコチラ→[【体験談】神里よしとさんのレッスンで、 Ruby完全初心者が対戦ゲームを作ってみた](https://www.mayuowl.com/ruby-first/)

## お題２　「戦う」か「逃げる」を選択できるようにする

まずは選択肢を表示して、そのコマンド入力を促します。HPの表示を指示したコードの下に以下のコードを書き足します。

System.out.println("\nどうする？ 1：たたかう 2：にげる"); System.out.print("コマンドを数字で入力→ ");

※ \n で改行

実行して様子を見てみます。実行する際は必ずファイルの保存→コンパイル（javac MiniGame.java）→実行（java MiniGame）の順番でやります。ターミナルでは↑↓キーで過去に使ったコマンドが呼び出せるので、直近で使ったコマンドは書かなくて済みます。

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-10.31.09.png)

これで選択肢とコマンド入力の誘導が表示できました。

次はプレーヤーからの入力を受け取る部分を書いていきます。Javaでは入力を受け取る方法がいくつかありますが、今回は以下のクラスとメソッドを使ってみます。

InputStreamReader isr = new InputStreamReader(System.in); BufferedReader br = new BufferedReader(isr); String str = br.readLine(); int choice = Integer.parseInt(str); System.out.println("ゆきたは " + choice + " を選んだ");

ものすご〜くざっくりと説明すると、１、２行目で入力データを受け取って、３行目で受け取った入力データをString（文字列）型の変数strに代入し、４行目でそれをさらに整数に変換してint型の変数choiceに代入しています。そして最後の行で、選んだコマンドを表示しています。

上記のコードをコマンド誘導の下に書き足して早速実行してみたいところですが、あと２つやらなければならないことがあります。１つはパッケージのインポート、もう１つは例外処理です。

InputStreamReaderクラスやBufferedReaderクラスはjava.ioパッケージで提供されているクラスです。このような外部のパッケージのクラスを利用する時は本来以下のようにパッケージ名 + クラス名で書く必要があります。

java.io.InputStreamReader isr = new java.io.InputStreamReader(System.in);

しかし何度もパッケージ名を書くのはうっとうしいので、前もってこのパッケージを使うよ！ということを宣言しておくことで、パッケージ名を省略することができるようになります。具体的にはソースコードの一番上に、利用するパッケージ名 + クラス名をimport文で指定します。

import java.io.InputStreamReader;

また

import java.io.\*;

と書くとjava.ioパッケージ内の全てのクラスの利用時にパッケージ名を省略できるようになります。今回はBufferedReaderやこのあと出てくるIOExecptionもjava.ioパッケージに含まれているのでこちらのインポート文を使ってみます。

もう１つの例外処理ですが、入出力を伴う処理を実行すると例えば文字コードが違うとか、読み込みたいファイルが存在しないなど想定外のことが発生する可能性があります。Javaではそうしたが例外が発生した時の処理を予め書いておくことになっています。具体的には例外が発生し得るメソッド内でtry~catchブロックを書くか、メソッドにthrowsで例外が発生し得ることを宣言するかです。

今回はtry~catchで書いてみます。

import java.io.\*; public class MiniGame { static int yukitaHp = 100; static int slimeHp = 100; public static void main(String[] args) { try { System.out.println("--ゆきたHP：" + yukitaHp + " --"); System.out.println("--スライムHP：" + slimeHp + " --"); System.out.println("\nどうする？ 1：たたかう 2：にげる"); System.out.print("コマンドを数字で入力→ "); InputStreamReader isr = new InputStreamReader(System.in); BufferedReader br = new BufferedReader(isr); String str = br.readLine(); int choice = Integer.parseInt(str); System.out.println("\nゆきたは " + choice + " を選んだ"); } catch(IOException e) { e.printStackTrace(); } } }

これまで書き足したものも含めて現状のコード全文です。ちょっと乱暴ですが今まで書いたコードをまるっとtryブロックで囲み、その下にcatchブロックを書いています。tryブロックの中の処理でIOExceptionが発生した場合はcatchブロックに移って例外処理が実行されます。

これでようやく実行できる状態になりました。コンパイルして実行してみます。

![](https://creatase.info/wp-content/uploads/2018/04/スクリーンショット-2018-04-20-13.50.48.png)

無事に入力したデータを受け取れたようです。これで「戦う」か「逃げる」かを選択できるようになりました。

次回はお題３以降にとりかかります。（また１個しかお題進まなかった…）

- [Javaでドラクエっぽい対戦ゲームを作ってみる【準備編】](https://creatase.info/java_minigame_1/)
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題１+寄り道編】](https://creatase.info/java_nimigame_2/)
- Javaでドラクエっぽい対戦ゲームを作ってみる【お題２編】←いまココ
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題３-４編】](https://creatase.info/java_minigame_4/)
- [Javaでドラクエっぽい対戦ゲームを作ってみる【お題５-7編】](https://creatase.info/java_minigame_5/)
