#1日目

## 登録まで
パスをもらうまでの作業はe-mailを指定してパスポートを提示するだけで簡単だったが
moscone WestをMoscone Southと勘違いしていて初回セッションにすこし遅刻してしまった


## Java Modules: Project Jigsaw Importance and Its Benefits for Developers
途中参加のため、ほとんど聴講時間なし。しかも45分のセッションが30分で切り上げられてしまった。

聴講者は部屋の大きさのわりに多め。やはりみんなProject Jigsawには興味があるようだ。
マネージャークラスよりも現場のリーダー的な立ち位置の年代が多いように見える。ほとんどスーツは着ていない。


## Microservices Evolution: Breaking Your Monolithic Database
DevOpaとMicloServiceは切っても切り離せない大事なものやで
メンテナンスウィンドウはんあんだこれ。
ブルーグリーンデプロイメントの話。プロキシで切り替えればいいんやでという話はわかるが、ゼロダウンタイムといってしまっていいのだろうか。
ゼロダウンマイグレーションがテーブル変更に対しても成り立つのはむずかしくないかと思った。

カラム追加、リネームからむ、タイプフォーマットの変更、カラムの削除

Columnのリネーム
追加→shardsを使ってコピーする→両書き込み、古いColumnから読み込み→了解込み、新しいのを読む→新しいColumnに読み書き→古いColumnの削除
Column削除
前提として、しちゃいけない→やるなら、読み込みはやめるが書き込みは続ける→削除する。

CQRS command query responsibility segregation
Write Operation
Read Operation

CQRSは新しくないが、MicroServiceを安全に試行するためには必要なステップである。

CQRSのためには様々なシナリオがある
view
  一番かんたんなやりかた。だけどパフォーマンスの問題にアンリ安い
materialized view
   広く使われれおり、パフォーマンスもわるくない
   Strong or Eventual Consistency
   One database must be reachable by the other
   DBlink
   Updatable(?)
mirror table using trigger
  Depends on database support
  Strong
  One database must be reachable by other

mirror table using transactional code
  any code
  stong consistendcy
  stored procedures or distributed transactions
  cohesion and cupling issue
  updatable(?)

Mirror Table using ETL
  Lots of available tools
  Requires external trigger (usually time-based)
  Can aggregate from multiple datasources
  Read Only

Event Sourcing
  State of data is a stream of events
  Eases auditing
  Eventual Consistency
  Distributable stream through a message bus

Debeziumがデータコピーの観点で有用なツールとして使えるとのこと（Redhat性のツールなので、宣伝臭くもすこしだけあった）

@yanagaさんの話し方が耳に入りやすく、話の流れもうまく作られていて更にイメージが湧いた。

# Let’s Talk About NOSQL
GXPの鈴木さんのセッションと思ったが、ほとんど喋らず。今回は開催側の要望でSpeaker枠にいたとこのと。

NoSQLの紹介。Writeは一つであとはReplicationだよという構造を
Document型、KeyValue型、Column型、Graph型、左記の複合型の紹介。かなり基本的な部分から紹介している。
それぞれの特徴と、どんな会社が使っているかを説明

View Tier
Logic Tier
  presentation
  application
  Business
  dataacess
DataSource

NoSQLにしたほうが、JDBCの載せ替えより構成が簡単になるからよいということが話したいみたい。

Spring Data
Hibernate OGM
TopLink
が現状のソリューション

JPAの問題
  非同期保存
  非同期Callback
  transactional
  Consistency Level
  SQL based
  Diversity in NoSQL

想定しているのはとDAOが低レベルAPIを呼ぶのではなく、もう一つプロトコルを挟んで
DAO - Hi Level API - LowLevelAPI - database という構成にしたいということ
そのためにあるのが Diana です。（お、製品紹介が来たぞ）

ロードマップとしてJSRに入れるところまで想定しているとのこと。

APIもとりあえずは一式揃えているようだ

## Reactive Microservices with Java and Java EE

2014年から参加し続けているよ

Monolith VS Microservice

強気というか席があるから座りなよ、そうじゃなきゃ出ていきなっていうスタイルはどうなのとは思う。

Spling Cloud - Spring Boot - NETFLIX OSSという技術スタックで紹介するとのこと
NetFlixのOSS盛りだくさんですね。

EurekaやRiboon、Hystrix、Zuulの紹介

Spring Cloud SecurityでOAuthしてまっせという話みたい。

AsycはJavaEEの中でも複数のスタック(EJB,JMS,CDI,CDI,Servlet,JAX-RS,)で実装されている

Project ReactorはFlux,MonoがReactiveComposableAPIとして利用可。なんかMQTTみたい。


## Java Keynote
火星開発の知見が高まった。というのは冗談としても、無駄に長過ぎたのでは。

jshellは補完が効くの嬉しい。

Java＋DockerでJavaが使いやすくなったよ。ライセンスの問題はどうなるのかしらないけど。

2017にJavaEE8を出して、2018にJavaEE9を出すよという発表をしていたが、
スピード麺での懸念はもちろん、Serverlessアーキテクトの話が出ていたので無謀では感がすごくある。
