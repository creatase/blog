---
title: プログラムを作ってみよう ズンドコ編 vol.2
date: 2018-06-13T18:50:08
description: さて今回から具体的に「ズンドコ」プログラム（ネーミングセンス…orz）を書いてみたいと思います。ちなみ
slug: lets_make_a_program_zundoko-vol-2
category: JavaProgramming
tags: 
keywords: 
---

こんにちは、ゆきたです。

さて今回から具体的に「ズンドコ」プログラム（ネーミングセンス…orz）を書いてみたいと思います。ちなみに私はJavaでやってますが、他の言語でもぜひやってみてくださいねー。そしてこの記事に書いているのはあくまでも一例です。これよりスマートな書き方はたっくさんあると思うのでいろんなアプローチを考えてみるのもいいですね。では本編をどうぞ〜。

## プログラムその１の内容

最初にプログラムその１の内容を確認しておきます。

- 「ズン」と「ドコ」をランダムに５回表示（出力）する。
- ５回分の「ズン」「ドコ」を繋いだ文字列が「ズンズンズンズンドコ」の場合に「キヨシ」を表示する。
- 「キヨシ」が表示されたらプログラムを終了する。

これを一つずつ実現していきます。

（今回全部ローカル変数でやっております。色々ツッコミどころはあるかと思いますがご容赦をば…）

## ランダムに５回表示

まず文字列の配列arrayを宣言して、表示する選択肢の「ズン」と「ドコ」を要素として代入しました。
```Java
String[] array = {"ズン", "ドコ"};
```
で、この配列からランダムで要素を指定して５回表示するというのを考えます。配列の要素はarray[j]で j 番目の値を指定できるので、この j の部分に０（→ズン）か１（→ドコ）をランダムに入れれば、「ズン」と「ドコ」をランダムに指定できそうです。

０か１をランダムに取得するのはjava.util.RamdomクラスのnextInt(int bound)メソッドを使いました。
```Java
public int nextInt(int bound)
```
このメソッドでは０から引数のbound – 1の整数をランダムに返します。なのでnextInt( 2 )とすれば０か１がランダムで返されます。また、見ての通りインスタンスメソッドなのでインスタンスを作って使用します。
```Java
Random r = new Random();
r.nextInt(2); //ランダムで0か1
```
これを先ほどの配列arrayの添字（インデックス）に突っ込みます。
```Java
array[r.nextInt(2)]; //ランダムでズンかドコ
```
あとはこれをループで５回繰り返せば良さそうです。今回はfor文を使ってみます。
```Java
for(int i = 0; i \< 5; i++) {
  System.out.print(array[r.nextInt(2)]);
}
```
他にもif文を使ったりswitch文を使ったり色々やり方があると思いますが、「ズン」か「ドコ」が５回表示されてれば問題ないと思います。とりあえずこれはこれで終わりにして次行きます。

## 正解の”ズンズンズンズンドコ”と比較

「ズン」と「ドコ」を５回ランダムで表示して「ズンズンズンズンドコ」だったら「キヨシ」を表示します。ということは「ズン」と「ドコ」をランダムに５回繋いだ文字列と正解の文字列を比較すれば良さそうです。というわけで、上では単に５回表示していた「ズン」「ドコ」を文字列として繋いでから表示してみます。

今回はStringBuilderクラスのappend(String str)メソッドを使いました。

もちろんStringで変数を宣言して += 演算子で「ズン」「ドコ」を足していっても問題ないと思います。
```Java
StringBuilder sb = new StringBuilder();
for(int i = 0; i < 5; i++) {
  sb.append(array[r.nextInt(2)]);
}
```
これで「ズン」「ドコ」を５回ランダムに繋げたので、toString()メソッドで文字列に変換して表示します。
```Java
String str = sb.toString();
System.out.println(str);
```
最後にequalsメソッドを使って正解の文字列と比較して、一致した場合に「キヨシ」を表示するようにします。
```Java
String correctStr = "ズンズンズンズンドコ";
if(str.equals(correctStr)){
  System.out.println("きよし！！！");
}
```
これでstrの中身が”ズンズンズンズンドコ”なら”きよし！！！”と表示されるようになりました。

次の過程のために一応ここまでをまとめておきます。
```Java
import java.util.Random;

public class Zundoko1 {
  public static void main(String[] args) {
    String correctStr = "ズンズンズンズンドコ";
    String[] array = {"ズン", "ドコ"};
    Random r = new Random();
    StringBuilder sb = new StringBuilder();
    for(int i = 0; i < 5; i++) {
      sb.append(array[r.nextInt(2)]);
    }
    String str = sb.toString();
    System.out.println(str);
    if(str.equals(correctStr)){
      System.out.println("きよし！！！");
    }
  }
}
```
## きよし！！！が出るまでループ

では最後、”きよし！！！”が出るまでループさせてみます。ちなみにこのプログラムの条件だと変数strの中身が”ズンズンズンズンドコ”になる確率は1/32。つまり約3％（結構低い…

さて、上のどの部分をループさせれば良いかを考えます。for文以下をループさせればいいかなぁと最初考えました。今回はwhile文を使って条件式にtrueを入れて”きよし！！！”が表示された場合にbreak文でループを抜けるようにしています。
```Java
while(true){
  for(int i = 0; i < 5; i++) {
    sb.append(array[r.nextInt(2)]);
  }
  String str = sb.toString();
  System.out.println(str);
  if(str.equals(correctStr)){
    System.out.println("きよし！！！");
    break;
  }
}
```
なんとなく良さそうに見えますが、これではまずいのです。なぜなら変数sbがループの最初で初期化されていないので、最初の１回で”きよし！！！”が出なければ、以降延々と「ズン」と「ドコ」が足され続けて無限ループになります。（なりました落ちました 笑

なのでループの最初に初期化処理を持って来ましょう。その他改行など入れると最終的にこんな感じになりました。（以下全文
```Java
import java.util.Random;

public class Zundoko1 {
  public static void main(String[] args) {
    String correctStr = "ズンズンズンズンドコ";
    String[] array = {"ズン", "ドコ"};
    Random r = new Random();
    StringBuilder sb;
    while(true){
      sb = new StringBuilder();
      for(int i = 0; i < 5; i++) {
        sb.append(array[r.nextInt(2)]);
      }
      String str = sb.toString();
      System.out.println(str);
      if(str.equals(correctStr)){
        System.out.println();
        System.out.println("きよし！！！");
        break;
      }
    }
  }
}
```
実行するとこうなります。（５回実行）

![](https://creatase.info/wp-content/uploads/2018/06/ズンズンズンズンドコ1-1.gif)

ちゃんと”きよし！！！”でループを抜けているようなのでこれでこのプログラムはよしとします。

さて、ここまでやってみて、「そうじゃないのよ」と感じましたよね。。。そうなんですよね、本来のお題はこういうことじゃないんですよね。。。

というわけで次回もう一つのプログラムを書いていきたいと思います。

ではでは〜また次回

