# 1日目

## 登録まで

パスをもらうまでの作業は、

1. 会場へ移動
2. カウンターでe-mailを指定
3. パスポートを提示

だけで簡単だった。パスがあればそれを別カウンターで提示することでバッグとTシャツももらえた。
バッグとTシャツについてはあせらないでホテルに戻る前に時間を見つけて受け取ったほうが良かったかも。（手荷物になったので）

## Java Modules: Project Jigsaw Importance and Its Benefits for Developers
moscone WestをMoscone Southと勘違いしていて初回セッションにすこし遅刻してしまった
途中参加のため、ほとんど聴講時間なし。しかも45分のセッションが30分で切り上げられてしまった。

聴講者は部屋の大きさのわりに多め。やはりみんなProject Jigsawには興味があるようだ。
マネージャークラスよりも現場のリーダー的な立ち位置の年代が多いように見える。ほとんどスーツは着ていない。


## Microservices Evolution: Breaking Your Monolithic Database
DevOpaとMicloServiceは切っても切り離せない大事なものということで、DevOps側の部分も懇切丁寧に解説。
ブルーグリーンデプロイメントの話もあり。プロキシで切り替えれば良いという話はわかるが、２世代以上の切り戻しが必要な場合も考えると
ゼロダウンタイムといってしまっていいのだろうかとは思う。

ゼロダウンマイグレーションがテーブル変更に対しても成り立つのはむずかしくないかと思った。
が、今回のセッションはまさにそのテーブル変更の話がメイン。話の持って行き方が非常に上手だった。

@yanagaさんの経験から、以下のシナリオが提示され、そのシナリオについて解説があった
- Column追加
- リネームColumn
- Columnのタイプフォーマットの変更
- カラムの削除

メモできたれた範囲だと以下の通り
- リネームColumn
 - Column追加
 - shardsを使って古いColumnから新しいColumnにデータをコピーする
 - 新旧Columnに書き込み、旧Columnから読み込み
 - 新旧Columnに書き込み、心Columnから読み込み
 - 新Columnに読み書き
 - 古いColumnの削除
- Column削除
 - 前提として、広報互換性を守るなら、削除しちゃいけない
 - やるなら、読み込みはやめるが書き込みは続ける
 - 削除すうる

- CQRS command query responsibility segregation
 - Write Operation
 - Read Operation

CQRSは新しくないが、MicroServiceを安全に試行するためには必要なステップである。

CQRSのためには様々なシナリオがある
- view
 - 一番かんたんなやりかた。だけどパフォーマンスの問題にアンリ安い
- materialized view
 - 広く使われれおり、パフォーマンスもわるくない
 - Strong or Eventual Consistency
 - One database must be reachable by the other
 - DBlink
 - Updatable(?)
- mirror table using trigger
 - Depends on database support
 - Strong
 - One database must be reachable by other
- mirror table using transactional code
 - any code
 - stong consistendcy
 - stored procedures or distributed transactions
 - cohesion and cupling issue
 - updatable(?)
- Mirror Table using ETL
  - Lots of available tools
  - Requires external trigger (usually time-based)
  - Can aggregate from multiple datasources
  - Read Only
- Event Sourcing
 - State of data is a stream of events
 - Eases auditing
 - Eventual Consistency
 - Distributable stream through a message bus

Debeziumがデータコピーの観点で有用なツールとして使えるとのこと（Redhat製のツール）

@yanagaさんの話し方が耳に入りやすく、話の流れもうまく作られていて更にイメージが湧いた。

## Let’s Talk About NOSQL

NoSQLの紹介からはじまる。
Document型、KeyValue型、Column型、Graph型、左記の複合型の紹介。かなり基本的な部分から紹介している。
それぞれの特徴と、どんな会社が使っているかを説明

- View Tier
- Logic Tier
 - presentation
 - application
 - Business
 - dataacess
- Data Access Tier

NoSQLにしたほうが、JDBCより構成が簡単になるからよいということが話したいみたい。

- Spring Data
- Hibernate OGM
- TopLink
が現状のソリューション

- JPAの問題
 - 非同期保存
 - 非同期Callback
 - transactional
 - Consistency Level
 - SQL based
 - Diversity in NoSQL

想定しているのはとDAOが低レベルAPIを呼ぶのではなく、もう一つプロトコルを挟んで
DAO - Hi Level API - LowLevelAPI - database という構成にしたいとのこと。

そのためにあるのが Diana というソフトウェアで、その仕様はロードマップとしてJSRに
入れるところまで想定している。

APIもとりあえずは一式揃えているようだ。

## Reactive Microservices with Java and Java EE

席は殆ど満席で立ち見の人にも開いた席に座るよう指示していた。
なるべく導線を開けておきたいということなんだろう。

Spling Cloud - Spring Boot - NETFLIX OSSという技術スタックで紹介するとのこと
NetFlixのOSS盛りだくさんなでもセッションというセッションだった。

デモコードの場所は
https://github.com/rcandidosilva/reactive-microservices

EurekaやRiboon、Hystrix、Zuulの紹介

Spring Cloud SecurityでOAuthしているという話みたい。

AsycはJavaEEの中でも複数のスタック(EJB,JMS,CDI,CDI,Servlet,JAX-RS,等々)で実装されている
Project ReactorはFlux,MonoがReactiveComposableAPIとして利用可。なんかMQTTみたい。

## Java Keynote

寒い。寒すぎた。

火星開発の知見が高まった。というのは冗談としても、無駄に長過ぎたのでは。

jshellは補完が効くの嬉しい。

Java＋DockerでJavaが使いやすくなったよ。ライセンスの問題はどうなるのかは言及なし。

2017にJavaEE8を出して、2018にJavaEE9を出すよという発表をしていたが、
スピード面での懸念はもちろん、Serverlessアーキテクトの話が出ていたので無謀では感がすごくある。

結局JavaEEのデモも時間足らずで打ち切り。火星の話もそうだけど、表示周りでかなりグダグダしてしまっていたので、
もう少しタイムキーパーがしっかりしてくれれば・・・！と思うことしきりなKeynoteだった。

## Java End-to-End Enterprise Applications with NetBeans HTML/Java APIs

Keynoteで体力を奪われた結果、少しこのセッションで眠ってしまったので、記載が曖昧。

人は想定していたとおりまばら。

View と Modelのバインドが頭に出てきた。

HTML を Javaとバインドする歩法

画面が小さくて何しているか皆目見当もつかない
双方向バインディングみたいなものか。

ParseJson＝Define ＠Model

DukeScript http://dukescript.com/ の話だった。

Java→JS→ブラウザ上のVM？が解釈して動かしてくれるよという話だった印象。
AngularJSの話は直接は出なかったが、最後にAngularJS使ってないの？という質問が出てきた。その回答としては
「AngularJSも素晴らしいんだけど、DukeScriptにはToo Muchなものなので、KnockoutJSを使っているよとのこと。

GAEのコンセプトもJavaで書いてHTMLで動いてるって言う感じだった気がするが、これは何が違うのか・・・。

## Enterprise Modeling of MVC and Java EE Artifacts

### MVC1.0の紹介

Action-based MVCというリクエストがコントローラーを呼び出してモデルとビューをアップデートするフレームワーク

クラス自体が、コントローラーだとしたら、メソッドがViewを返すもの、
Modelはクラスがフィールドで保持するデータのこと。

以下の形式でViewを返す
- return new Viewable(～)
- return Response.status(OK).entity～

引数で@Valid嬉しい。@BeanParamはフォームとのヒモ付のことかな？@CsrfValidはそれだけでCsrfちぇっくしてくれるのか。嬉しい。

### JPA Modeler
- Generate SQL
- Generate Code
等の機能あり

素晴らしいツールのようだけど、AmaterasERDとの比較が気になる。
とくに、結局変更管理が追いにくいと本末転倒なので。

###MVCPlugin
NetBeansにあるよ

今は作っている途中だから機能はシンプルだけど、コメントやフィードバックを受けつけているとのこと。
