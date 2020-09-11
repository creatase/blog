---
title: JavaEE6 Webコンポーネントディベロッパー 【まとめノート】3
date: 2018-05-08T18:18:05
description: 試験まであと４日です。どこまでまとめられるかわかりませんが続けていきたいと思います。
slug: javaee6_wcd_study_note_3
category: Java
tags: 
keywords: 
---

こんにちは、ゆきたです。

試験まであと４日です。どこまでまとめられるかわかりませんが続けていきたいと思います。

## Declarative Security

宣言的セキュリティ。デプロイメント記述子に記述するロール、アクセス制御、認証の要求事項について。

合わせてアクセス制御アノテーションについても理解しておく。

こちらの記事が参考になる。→[Java EE 6: アプリケーション セキュリティの強化](https://www.infoq.com/jp/news/2010/07/javaee6-security)

## Annotations and Resource Injection

### @DeclareRoles

セキュリティロールの指定に使用。@RolesAllowedで宣言されたロールは＠DeclareRolesで宣言する必要はない。
```Java
@DeclareRoles("Admin") // Adminというロールの利用を宣言
```
これはデプロイメント記述子の中で以下のように定義するのと等価。
```xml
<web-app>
  <security-role>
    <role-name>Admin</role-name>
  </security-role>
</web-app>
```
### @EJB

デプロイメント記述子でのejb-refまたはejb-local-ref要素を宣言するのと等価。
```Java
@EJB private ShoppingCart myCart;
```
### @EJBs

単一のリソース上で複数の＠EJBを宣言する際に使用。
```Java
@EJBs({@EJB(Calculator), @EJB(ShoppingCart)})
```
### @Resource

デプロイメント記述子でのresource-ref、message-destination-ref、env-ref、resource-env-ref要素の宣言と等価。クラス、メソッドあるいはフィールド上で指定する。
```Java
@Resource private javax.sql.DataSource catalogDS;
public getProductsByCategory() {
    // get a connection and execute the query
    Connection conn = catalogDS.getConnection();
..
}
```
この例ではjavax.sql.DataSource型のcatalogDSを宣言。

### @PersistenceContext

EntityManagerをインジェクトする。
```Java
@PersistenceContext
EntityManager em;
```
### @PersistenceContexts

複数の@PersistenceContextの宣言時に使用。

### @PersistenceUnit

EntityManagerFactoryをインジェクトする。
```Java
@PersistenceUnit
EntityManagerFactory emf;
```
### @PersistenceUnits

複数の@PersistenceUnitの宣言時に使用。

### @PostConstruct

依存性注入後に初期化のために実行する必要のあるメソッドに対して使用。引数はなし、戻り値はvoidでなければならない。
```Java
@PostConstruct
public void postConstruct() {
  ...
}
```
### @PreDestroy

インスタンスがコンテナにより削除処理中であることを知らせるためのコールバック通知としてメソッドで使用。戻り値はvoid。
```Java
@PreDestroy
public void cleanup() {
  // オープンなリソースのクリーンアップ
  ...
}
```
### @Resources

複数の@Resourceアノテーションのコンテナとして機能する。
```Java
@Resources ({
@Resource(name=”myDB” type=javax.sql.DataSource),
@Resource(name=”myMQ” type=javax.jms.ConnectionFactory)
})
```
### @RunAs

デプロイメント記述子の中のrun-as要素と等価。
```Java
@RunAs(“Admin”)
public class CalculatorServlet {
  ...
}
```
```xml
<servlet>
    <servlet-name>CalculatorServlet</servlet-name>
    <run-as>Admin</run-as>
</servlet>
```
### @WebServiceRef

デプロイメント記述子の中のresource-ref要素と同じ。
```Java
@WebServiceRef private MyService service;
```
### @WebServiceRefs

複数の@WebServiceRefアノテーションを可能にする。

ひとまず手がかりぐらいはまとめられたかと思います。読者の参考になれば幸いです。

- [JavaEE6 Webコンポーネントディベロッパー 【まとめノート】１](https://creatase.info/javaee6_wcd_study_note_1/)
- [JavaEE6 Webコンポーネントディベロッパー 【まとめノート】２](https://creatase.info/javaee6_wcd_study_note_2/)
- JavaEE6 Webコンポーネントディベロッパー 【まとめノート】３←いまココ
