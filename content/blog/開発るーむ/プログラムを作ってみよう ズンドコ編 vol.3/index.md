---
title: プログラムを作ってみよう ズンドコ編 vol.3
date: 2018-06-14T18:50:44
description: 気がつけばこの「ズンドコ」プログラムも３回目。今回は多分これが本命の２つ目のプログラムを書いていってみ
slug: lets_make_a_program_zundoko-vol-3
category: JavaProgramming
tags: 
keywords: 
---

こんにちは、ゆきたです。

&nbsp;

気がつけばこの「ズンドコ」プログラムも３回目。今回は多分これが本命の２つ目のプログラムを書いていってみたいと思います。（なお前回同様書き方など至らぬ点は何卒ご容赦くださいませm(\_ \_)m　優しくご指導いただければ幸いです。。）

## プログラムその２の内容

まずはプログラムその２の内容を確認しておきます。

- 「ズン」と「ドコ」をランダムに表示（出力）する。
- 表示された「ズン」または「ドコ」を含めて過去５回分の「ズン」「ドコ」を文字列として保持・更新する。
- 過去５回分の「ズン」「ドコ」の文字列が「ズンズンズンズンドコ」の場合に「キヨシ」を表示する。
- 「キヨシ」が表示されたらプログラムを終了する。

さぁ前回同様に一つずつ消化していきたいと思います。が、先に言っておくと２つ目の、直近５回分の「ズン」「ドコ」の文字列をどうやって保持・更新しようかというところがポイントでした。これについては配列を使うとか方法は色々あるのだろうと思いますが、今回私はコレクションを使うことにしました。理由はそのほうがメモリ食わないかなぁと思ったからです。では本編をどうぞ〜。

&nbsp;

## 「ズン」「ドコ」をランダム表示

これはプログラムその１で使ったコードを流用できそうなので、使えるところはそのまま使います。詳細は前回記事をご覧ください。→[プログラムを作ってみよう ズンドコ編 vol.2](https://creatase.info/lets_make_a_program_zundoko-vol-2)

String[] words = {"ズン", "ドコ"}; Random r = new Random(); word = words[r.nextInt(2)]; System.out.print(word); 

printメソッドにwrods[r.nextInt(2)]を直接渡していないのは、このタイミングで選ばれた「ズン」か「ドコ」を次の工程でもう一度使うためです。

&nbsp;

## 直近５回の「ズン」「ドコ」履歴を保持・更新する

上の処理をループさせれば「ズン」か「ドコ」がランダムに表示され続けます。まず、上の処理で表示した文字を５回分ストックすることを考えます。ストックする入れ物として具体的にはLinkedListを使いました。LinkedListは配列と違い格納できる要素数が可変で、要素を自由に挿入したり削除したりできます。Listの仲間にはArrayListもありますが、今回は要素の挿入・削除がメインになるのでその処理がArrayListより早いLinkedListを使っています。このLinkedListのインスタンスを作り、上の処理で表示した「ズン」「ドコ」をadd()メソッドで表示した順に挿入していきます。

List\<String\> list = new LinkedList\<\>(); list.add(word); 

で、「ズン」か「ドコ」を挿入した結果、listの要素数が５より大きくなった場合、remove(0)メソッドで、listに挿入されたタイミングがもっとも古い（０番目の）要素を削除します。

if(list.size() \> 5) { list.remove(0);} 

これでlistの中には「ズン」「ドコ」が表示された順に直近５回分が格納されていることになります。

&nbsp;

## listを文字列へ変換して”きよし！！！”判定

それではlistの中に格納されている要素を文字列に変換して、”ズンズンズンズンドコ”と比較します。listから文字列への変換はストリームAPIを使ってみました。ストリームAPIは配列やコレクションなどを元に集計操作を行うAPIです。（ストリームAPI以外にも手はもちろんあります）

String str = list.stream().collect(Collectors.joining());

詳細な説明は割愛しますが、stream()メソッドでストリームを生成、collect()メソッドでリダクション操作（入力要素をもとに結合操作を繰り返し実行して、単一の結果を得る操作：最大値を返す、要素数を返すなど）、その引数にCollectors.joining()（→戻り値は入力要素を検出順に連結して１つのStringにするCollector）を渡しています。

要するにlistに入っている５つの要素が入ってる順に結合された文字列がstrに代入されます。（乱暴…

続いてstrに代入した文字列と正解の文字列を比較します。この部分もプログラムその１で書いた処理を流用できるのでこれを使います。

String correctStr = "ズンズンズンズンドコ"; if(str.equals(correctStr)){ System.out.println("きよし！！！");}

これで表示された直近５回分の文字列が”ズンズンズンズンドコ”の場合”きよし！！！”と表示されるようになりました。この処理は「ズン」か「ドコ」が１回表示されるたびに実行されるようにループの中に入れます。

&nbsp;

## きよし表示でループを抜ける

さて、ここまで出来ればあとはループ処理を書いておしまいです。どの部分をループさせるかを考えます。今回も前回同様while文の条件式にtrueを入れて無限ループにし、”きよし！！！”が表示された直後にbreak文でループを抜けるようにしました。以下全文です。（適当に改行入れてます。。

import java.util.LinkedList;import java.util.List;import java.util.Random;import java.util.stream.Collectors;public class Zundoko2 { public static void main(String[] args) { System.out.print("\n\n\n\n"); // \nは改行&nbsp; &nbsp; &nbsp; &nbsp; String correctStr = "ズンズンズンズンドコ";&nbsp; &nbsp; &nbsp; &nbsp; String[] words = {"ズン", "ドコ"};&nbsp; &nbsp; &nbsp; &nbsp; List\<String\> list = new LinkedList\<\>();&nbsp; &nbsp; &nbsp; &nbsp; Random r = new Random();&nbsp; &nbsp; &nbsp; &nbsp; String str = "";&nbsp; &nbsp; &nbsp; &nbsp; String word = "";&nbsp; &nbsp; &nbsp; &nbsp; while(true) {&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; word = words[r.nextInt(2)];&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; System.out.print(word);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; list.add(word);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if(list.size() \> 5) { &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; list.remove(0);&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; str = list.stream().collect(Collectors.joining());&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; if(str.equals(correctStr)){ &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; System.out.println("\n\nきよし！！！\n\n\n\n");&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; break;&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; }&nbsp; &nbsp; &nbsp; &nbsp; } }}

実行したものがこちら。（５回実行）

![](https://creatase.info/wp-content/uploads/2018/06/ズンズンズンズンドコ2.gif)

案外こっちの方が早めに”きよし！！！”が出る気がしますね。こっちの確率は…計算の仕方がわかりません…（汗

一応これでお題はクリアしてるんじゃないかと勝手に思っています。そして今回は運よくできたので記事にしました。お題が全然違う内容だったらそもそもやってなかったかもしれません。

でもやってみてよかったと思います。というか本来プログラミングは今回のようにお題があってそれをどう実現するかを考えて行くものなんだなというのが実感できました。皆さんはいかがだったでしょうか？

&nbsp;

## 気をつけたこと

今回この記事を書くにあたって、単にコードの紹介をするのではなく、書く前に私が何を考えて、どうしようとしてコードを選んだかという部分をなるべく書くようにしました。

今思えば正規表現使えばもうちょっとスッキリしたかな？とか、そもそも文字列で比較すること自体がナンセンスか？といろいろ疑問が生まれてきます。が、とりあえずは「ボロボロでもいいのでなんとか動くものを書く」というのを目標にやってみました。読者の何かのお役に立てば幸いです！

拙い記事に最後までお付き合いいただきありがとうございました。

ではでは〜

