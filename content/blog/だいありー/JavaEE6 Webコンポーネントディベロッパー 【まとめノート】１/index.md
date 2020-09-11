---
title: JavaEE6 Webコンポーネントディベロッパー 【まとめノート】１
date: 2018-05-05T18:13:46
description: ただいまJavaEE6 Webコンポーネントディベロッパーの勉強中です。認定されている参考書がJava
slug: javaee6_wcd_study_note_1
category: Java
tags: 
keywords: 
---

こんにちは、ゆきたです。

ただいまJavaEE6 Webコンポーネントディベロッパーの勉強中です。認定されている参考書がJavaEE5までの内容となっていたので、EE6で新たに追加された内容の分について自分なりにまとめておこうと思います。

## 書籍など

ちなみにJavaEE５の試験範囲の学習にはこちらを使っています。

[![](//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&MarketPlace=JP&ASIN=4798121606&ServiceVersion=20070822&ID=AsinImage&WS=1&Format=_SL250_&tag=yukita2a01-22)](https://www.amazon.co.jp/gp/product/4798121606/ref=as_li_tl?ie=UTF8&camp=247&creative=1211&creativeASIN=4798121606&linkCode=as2&tag=yukita2a01-22&linkId=ac52b7cac2626128621e6f65c46977d2) ![](//ir-jp.amazon-adsystem.com/e/ir?t=yukita2a01-22&l=am2&o=9&a=4798121606)

[SUN教科書 Webコンポーネントディベロッパ(SJC-WC)(試験番号:310-083)](https://www.amazon.co.jp/gp/product/4798121606/ref=as_li_tl?ie=UTF8&camp=247&creative=1211&creativeASIN=4798121606&linkCode=as2&tag=yukita2a01-22&linkId=ddf0f9507e7db46bf42ed63eeb1f76c3) ![](//ir-jp.amazon-adsystem.com/e/ir?t=yukita2a01-22&l=am2&o=9&a=4798121606)

JavaEE6のドキュメント（英語）はこちらになります。

→[Java Servlet Specification, Version 3.0](http://download.oracle.com/otn-pub/jcp/servlet-3.0-fr-eval-oth-JSpec/servlet-3_0-final-spec.pdf)

一応日本語のドキュメントもありますが、これを読んで意味がわかるのは、すでにあらかた内容を理解している方だろうと思います。興味のある人は「サーブレット3.0」などで検索してみてください。（この記事を執筆中にはなぜかアクセスできなかった…。）

この仕様書の中で新たに試験範囲として押さえておくべき内容は以下となります。

- 2.3.3.3 Asynchronous processing ：非同期処理
- 3.2 File upload ：ファイルアップロード
- 8 Annotations and pluggability ：アノテーション／プラッガビリティ
- 13.2 Declarative Security :宣言的セキュリティ
- 15.5 Annotations and Resource Injection :アノテーション／リソースインジェクション

※こちらの記事を参考にしました。→参考記事 [体験記/Oracle Certified Expert, Java EE 6 Web Component Developer (1Z0-899) 資格取得](http://d.hatena.ne.jp/penguinwatcher/20140831/1409484117)

これに加えて、JavaEE7の書籍になるのですが、こちらも合わせて読みました。

[![](//ws-fe.amazon-adsystem.com/widgets/q?_encoding=UTF8&MarketPlace=JP&ASIN=4774183164&ServiceVersion=20070822&ID=AsinImage&WS=1&Format=_SL250_&tag=yukita2a01-22)](https://www.amazon.co.jp/gp/product/4774183164/ref=as_li_tl?ie=UTF8&camp=247&creative=1211&creativeASIN=4774183164&linkCode=as2&tag=yukita2a01-22&linkId=512d5398f5f91fdc689dfd4b49462aab) ![](//ir-jp.amazon-adsystem.com/e/ir?t=yukita2a01-22&l=am2&o=9&a=4774183164)

[パーフェクト Java EE](https://www.amazon.co.jp/gp/product/4774183164/ref=as_li_tl?ie=UTF8&camp=247&creative=1211&creativeASIN=4774183164&linkCode=as2&tag=yukita2a01-22&linkId=575932e0f878606eb6d3fc5a2d0a223f) ![](//ir-jp.amazon-adsystem.com/e/ir?t=yukita2a01-22&l=am2&o=9&a=4774183164)

## Asynchronous processing

非同期処理を使うには明示的な指定が必要。

＠WebServletアノテーションにはasyncSupported属性がありデフォルト値はfalseとなっている。これをasyncSupported=trueと指定する。
```Java
＠WebServlet(urlPatterns = "path", asyncSupported=true)
```
サーブレットの中（doXXXメソッド内）で、HttpServletRequestオブジェクトのstartAsyncメソッドによりAsyncContextオブジェクトを取得する。取得したAsyncContextオブジェクトに対し、startメソッドを実行することでstartメソッドの引数に渡したラムダ式（Runnableオブジェクトのrunメソッド：Runnableインターフェースを実装した別クラスを作っても良い）が別スレッド（非同期処理用スレッド）で実行される。
```Java
AsyncContext asyncCtx = request.startAsync(request, response);
asyncCtx.start(() -> { /*ここに非同期処理用スレッドで実行する処理を書く*/ });
```
サーブレット自体は処理を非同期処理用スレッドに移譲して終了するので、別のリクエストの処理が可能になる。

一方処理を移譲された非同期処理用スレッドは処理の結果を生成、ビューなどへフォワードするなどして、最後にAsyncContextオブジェクトのcompleteメソッドを呼びスレッドを完了する。
```Java
非同期処理内容
  //時間のかかる処理など
  //フォワード処理
  asyncCtx.complete(); //非同期処理用スレッドの完了
```
フォワード処理のみを非同期処理化することも可能。その場合はAsyncContextオブジェクトのdispatchメソッドを使う。この場合はcompleteメソッドの呼び出しは不要。
```Java
AsyncContext asyncCtx = request.startAsync(request, response);
asyncCtx.dispatch("path");
```
非同期処理を実装したサーブレットの前後にフィルタを入れる場合はそのフィルタにも明示的な指定が必要。＠WebFilterアノテーションにasyncSupported=trueと指定する。指定しないとエラーになる。
```Java
@WebFilter(urlPatterns = "path", asyncSupported=true)
```
また、非同期処理後のディスパッチにフィルタを適用する場合はフィルタの＠WebFilterアノテーションにdispatcherTypes=DispatcherType.ASYNCと指定するか、web.xmlの\<filter-mapping\>内に\<dispatcher\>ASYNC\</dispatcher\>と指定する。
```Java
アノテーションで指定
@WebFilter(
  urlPatterns = "path",
  asyncSupported = true,
  dispatcherTypes = DispatcherType.ASYNC
)
```
```xml
web.xmlで指定
<filter-mapping>
  <filter-name>name</filter-name>
  <url-pattern>path</url-pattern>
  <dispatcher>ASYNC</dispatcher>
</filter-mapping>
```
非同期処理の各タイミングで処理を実行するリスナを作成可能。AsyncListenerインターフェースを実装したリスナクラスを用意する。
```Java
private static class MyListener implements AsyncListener {
  @Override
  public void onComplete(AsyncEvent event) throws IOException { 処理 }
  @Override
  public void onError(AsyncEvent event) throws IOException { 処理 }
  @Override
  public void onStartAsync(AsyncEvent event) throws IOException { 処理 }
  @Override
  public void onTimeout(AsyncEvent event) throws IOException { 処理 }
}
```
- onCompleteメソッド：処理完了時
- onErrorメソッド：処理に失敗した時
- onStartAsyncメソッド：非同期処理の中で別の非同期処理を開始した時
- onTimeout：処理が指定時間内に終わらない時

非同期処理を発生させるサーブレット内（doXXXメソッド内）でリスナクラスのオブジェクトをAsyncContextオブジェクトのaddListenerメソッドの第１引数に渡してセットする。
```Java
AsyncContext asyncCtx = request.startAsync(request, response);
asyncCtx.addListener(new MyListener(), request, response);
asyncCtx.start(() -> { /*ここに非同期処理用スレッドで実行する処理を書く*/ });
```
多分全然内容足りてないけど主要なとこだけピックアップしています。

こちらの記事が大変参考になります。→[Servlet標準の非同期処理に触れてみる(Qiita)](https://qiita.com/kazuki43zoo/items/8be79f98621f90865b78)

続きは次回にします。

- JavaEE6 Webコンポーネントディベロッパー 【まとめノート】１←いまココ
- [JavaEE6 Webコンポーネントディベロッパー 【まとめノート】２](https://creatase.info/javaee6_wcd_study_note_2/)
- [JavaEE6 Webコンポーネントディベロッパー 【まとめノート】３](https://creatase.info/javaee6_wcd_study_note_3/)
