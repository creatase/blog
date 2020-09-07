---
title: JavaEE6 Webコンポーネントディベロッパー 【まとめノート】2
date: 2018-05-07T19:20:52
description: 前回に引き続きJavaEE5→6の差分ノートをまとめていきます。
slug: javaee6_wcd_study_note_2
category: Java
tags: 
keywords: 
---

こんにちは、ゆきたです。

前回に引き続きJavaEE5→6の差分ノートをまとめていきます。

## File upload

ファイルをアップロードするHTMLでは\<form\>タグにenctype属性指定が必要。

\<html\> \<body\> \<form action="upload" enctype="multipart/form-data" method="POST"\> アップロード： \<input type="file" name="file"\> \<input type="submit" value="Submit"\> \</form\> \</body\> \</html\>


ファイルのアップロードを受け取るサーブレットクラスには、＠MultipartConfigアノテーションを付与する。

@WebServlet("/upload") @MultipartConfig(location="/tmp", maxFileSize=100000)

@MultipartConfigアノテーション内の要素はオプション。locationはファイルを一時的に保存する保存先を指定。maxFileSize属性は最大ファイルサイズを指定（バイト）、デフォルト値は上限なしを意味する-1。明示的に指定したサイズを実ファイルサイズが越えていると、HttpServletRequestオブジェクトのgetPartsメソッドの呼び出し時にIllegalStateExceptionがスローされる。

input要素が複数ある場合、HttpServletRequestオブジェクトのgetPartsメソッドでPartオブジェクトのコレクションを取得する。

Collection\<Part\> parts = request.getParts();

単体の場合はgetPartメソッドで取得。

Part part = request.getPart("file");


Partオブジェクトのwriteメソッドで一時指定したディレクトリ（この場合は/tmp）へ保存する。

part.write(filename);

どうやらサーブレット3.0当時はこのfilenameを取得するのに一手間かかっていたよう。。その他メソッドなどとともに割愛します。ちなみにサーブレット3.1（JavaEE7）ではfilenameをあっさり取得するgetSubmittedFilenameという便利なメソッドが追加されています。

## Annotation and pluggability

サーブレット3.0で新たに定義されたアノテーションは、WEB-INF/classesディレクトリ内かWEB-INF/lib内に置かれたjarファイルにパッケージされている場合に有効。

web.xmlの\<web-app\>内のmetadata-complete属性がfalseの場合、上記のアノテーションが有効。trueを指定すると無効になる。

### ＠WebServlet

サーブレットの定義に使用。urlPatterns属性またはvalue属性のどちらかが必要。複数のURLをコンマ区切りで指定可能。その他の属性の指定は任意。URL以外の属性も指定する際はurlPatterns属性の使用が推奨される。また、＠WebServletアノテーションをつけたクラスはjavax.servlet.http.HttpServletクラスの継承が必須。

@WebServlet("/foo") //value省略可

@WebServlet(name="MyServlet", urlPatterns={"/foo", "/bar"})

### @WebFilter

フィルターの定義に使用。urlPatterns属性、servletNames属性あるいはvalue属性の指定が必要。その他の属性の指定は任意。URL以外の属性も指定する際はurlPatterns属性の使用が推奨される。また、@WebFilterアノテーションをつけたクラスはjavax.servlet.Filterインターフェースの実装が必須。

### @WebInitParam

WebServletとWebFilterアノテーションの属性の一つ。サーブレットやフィルタに初期化パラメータを渡す際に使用する。

@WebServlet(name="MyServlet", urlPatterns="/foo", initParams={@WebInitParam(name="name", value="Name")})

### @WebListener

リスナの定義に使用。@WebListerを付与したクラスには以下のインターフェースのどれかを実装している必要がある。

- javax.servlet.ServletContextListener
- javax.servlet.ServletContextAttributeListener
- javax.servlet.ServletRequestListener
- javax.servlet.ServletRequestAttributeListener
- javax.servlet.http.HttpSessionListener
- javax.servlet.http.HttpsessionAttributeListener

### @MultipartConfig

ファイルのアップロードを受ける場合に使用。詳細は本記事の「File upload」を参照。

### web.xmlとweb-fragment.xml

複数のフレームワークを利用する場合、それぞれのフレームワークの設定もweb.xmlに記述してくとweb.xmlが肥大化し、可読性が低下する。web-flagment.xmlにそれぞれのフレームワークの設定を記述し、管理する。

こちらの記事がわかりやすい。

参考記事→[Servlet 3.0 web-fragment.xml による設定](https://yoshio3.com/2010/03/14/servlet-3-0-web-fragment-xml-%E3%81%AB%E3%82%88%E3%82%8B%E8%A8%AD%E5%AE%9A/)


続きは次回。

- [JavaEE6 Webコンポーネントディベロッパー 【まとめノート】１](https://creatase.info/javaee6_wcd_study_note_1/)
- JavaEE6 Webコンポーネントディベロッパー 【まとめノート】２←いまココ
- [JavaEE6 Webコンポーネントディベロッパー 【まとめノート】３](https://creatase.info/javaee6_wcd_study_note_3/)
