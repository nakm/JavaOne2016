# 3日目
朝早く起きたものの、少しグダグダしてしまってセッション開始ギリギリに到着。

## Modern Web Apps with HTML5 Web Components, Polymer, JAX-RS, and Java EE MVC 1.0
スライドのタイトルが "Modern Web Apps with HTML5 Web Components, Polymer, Java EE MVC ,JAX-RS"になっていた。
昨日のMVC1.0のDropを受けて変えたのかなと少し勘ぐったが、会場から質問があった際に「こっちも驚いたよ」
「もしMVC1.0を気に入ったらフィードバックよろしくな」と言っていたので、やはり寝耳に水だった様子。

デモのコード https://github.com/kito99/polymer-javaee-mvc-todo をベースに要素技術である
MVC1.0やWebComponentの考え方を解説。
特にWebComponentについては、その定義をDropBoxの画面を基に解説。再利用の面で考えられてきた概念であり、
今までいろんな言語で用いられたという歴史を解説。

jQueryUIやBootStrup、JSFの画面を例に、抽象化と戦ってきた経験からWebComponentがこれらを解決するための手段になりうると説明した。

- WebComponent == Collection of HTML 5 Standards
 - custom Elements
 - HTML Template
 - HTML Imports
 - Shadow DOM
  - (Custom CSS Properties)

PolymerはGoogle製のWebComponentのJavscriptのライブラリで以下の特徴を持つ
- Simplefied model
- two-way databinding
- declative event handring
- behaviors
- property obserbation
ChromePlayやYoutubeなんかでも使われているようす。

MVC1.0のコードはそこそこにShadwo DOM等の画面側の解説が多く、コード解説の時間の7割程度はJavaScriptの解説だったように感じた。
その上2時間のはずのが1時間ちょっとで終わってしまったので、少し消化不足感は否めなかった。

会場のからの質問
- AngularJS使わないの？
 - 今の技術スタックでやれているので、試していないよ
- MVC1.0が死んだらWebComponentとデータバインドとかもできないの？
 ンいや、別の手段で全然できるんだけど、昨晩僕もDropを聞いたばかりだンら回答を用意する時間が・・・（会場が沸く）
- polymerでSPAできないの？
 - Backboneと組合わせればできるかもだけど、あんまりおすすめしない。別の目的のものだから。
- (セッションの中で紹介していた）HTML5インポート使わないの？
 - Polymerの機能の中で使ってるよ。
- HTML5サポートしていないブラウザはどうすればいい？
 - IEなら9移行であればX-Tagが使えるよ。それ以前のバージョンを使っているなら上げるしかない。

スピーカの方は、経験が豊富で実績がないことについてもしっかり調べていたので、ある程度技術領域を守りつつ周辺技術収集も怠らない真面目な方なんだなと感じた。WebComponent自体は個人的には触れた機会がなかったし、個人的なやることリストの中にも入らなさそうだけど、目的に対する製品スタック等参考になる部分は多々あった。

## Migrate Java EE Applications to Microservices and Oracle Java Cloud
デモのソースコードは　https://github.com/TFaga/kumuluzee-examples

"No long-term commitmtnet to a single stack"と最初に提示されており、個人的には大いに納得しつつもエンタープライズで乗り越えなきゃいけない大きな壁の一つだよなと思って聴講していた。

大まかにはモノリスなアプリケション構成(JSF+CDI → FacadeEJB＋EJB → JPA)から
(JSF CDI) →SOAP/REST→ (JAX-RS→FacadeCDI＋CDI→JPA)*という構成を経て
(HTML5) →SOAP/REST→ (JAX-RS→FacadeCDI＋CDI→JPA)*という構成にたどり着いたとのこと。

CQRSとか考えなければこうなるよなーと思いながら聴講していた。

AP間の非同期通信はMQ経由で実施し、Local/RemoteなEJB呼び出しはREST/SOAPに置き換えたとのこと。

ネットワークの通信量が増えるのどうするの、セキュリティはどうなるの、Session情報はどうするの等の質問が出たけど回答について行けずよく聞き取れなかった。

マイクロサービス化についてははフレームワークをkumuluzEE https://ee.kumuluz.com/ に置き換えたことで、パッケージング、デプロイ、スケール等々で救われたとのこと。具体的な解説は聞き取れなかったのが悔やまれる。

（最後までOracleCloudを使った明確な理由は理解できなかったけど、とりあえず使えたので使った。kumuluzEEを使えばクラウドへの置き換えも比較的楽ということなので、最初に提示されたとおり"No long-term commitmtnet to a single stack"ということなのかな。）

## JAX-RS 2.1 for Java EE 8

この日個人的には一番聞いてて楽しかったセッション。

jaersyは今後もリファレンス実装としての立ち位置は変えないよとのこと。

JAX-RS 2.1 JSR370 は2017のリリース予定。Non-blocking/IO,ReactiveClient,ServerPush等を導入する。

NonBlockingなI/Oについて、現状JAX-RSはロッキング前提で設定されており、MessageBodyReaderと
MessageBodyWriterを自体を変更する必要があるが、まだ明確なアイディアが出てないとのこと。

Reactive Client APIやNonBlockingI/O、ServerSentEventについての、細かな実装に対する検討事項の列挙や方針が示された。
特にReactiveClientAPIについては、仕様としてはCompetableFute<T>のサポートは必須としてほしいけど、実装側で提供する分には追加してサポートするものを増やすこともできるよとのこと。（この観点が何処につながっていくか、個人的には理解できなかったけど重要っぽいので記載）

また、他のJaveEEの仕様(スライドに出ていたのがJSON-B,CDI,Configuration)ともうまくやっていくよという説明と方針説明もあり、スピーカの熱意も十分感じられた。

最後にCircuit breakersの話が出てきており実装の方針を検討しているという説明をして、プレゼン終了。順番がなんかおかしい気がしたので急に足したのかな。

MVCがdropになったことで急にやることてんこ盛りになった感があったものの、利用者を安心させるために、出せる情報は出し尽くしたよという印象をスライド全体から受けた。

## Live-Coding No-Ceremony Microservices

レベルと画面の大きさが上級者過ぎたので、結局20分程度聴講してJavaHubに移動した。

狭い会場で20人ぐらいが膝を突き合わせてやるのであれば、内容的には悪くないと思ったが…。

## Project Jigsaw: Under The Hood
Jigsawは事前学習がない中で聴講したので、全くついて行けず。フードの下はまだフードだった感がある。
自分乗り会としては、publicがpublic過ぎたので、publicのなかで少し分類をするためにモジュールという概念を導入するよというものだと思っている。

ネットに転がってた内容と#j1jpハッシュタグのTLを見てまとめたもの。集合知ありがたい。
　　- Class Loader
 - モジュールを読み込む単位。
 - モジュールはカプセル化のためにはクラスローダーより良い仕組みである
   - が、まだ、クラスローダー自体は必要
   - なぜならクラスローダーはモジュールをグラフとして扱っており、循環参照やパッケージ分割などを防いでいるためである
- Layer
 - モジュールとクラスローダーの関係をコントローするもの
- Named modules
 - 他のモジュールにアクセス可能
- Unnamed modules
 - 他のモジュールにアクセス不可
- Automatic modules
 - Unnamed moduleをNamedModuleに昇格させるための仕組み。
- exports
 - publicで他のモジュールから呼び出し可能なパッケージを明示する
 - Reflectionでprivateメソッドの**実行不可**
- exports private
 - publicで他のモジュールから呼び出し可能なパッケージを明示する
 - Reflectionでprivateメソッドの**実行可**
- require
 - どういったモジュールに依存しているのかを明示する
- require transitive
 - どういったモジュールに依存しているのかを明示する
 - さらにこのモジュールを呼び出す元側にも依存関係を知らせる（呼び出し元でrequireを省略できる。）

require transitiveのユースケースがイマイチ思いつかないんだけど、loggingやjdbc等の
仕様と実装が別パッケージになるようなパターンで必要なもの？

## Toward an Evolutionary Design and Architecture [CON1062]
アジェンダのみ表示

- What is Architecture
- The risk no Eveloving
- The risk Eveloving
- how to deal with it
  - prioritize on value and Architecture Impact
- keep it Simple
  - Make it work, make it better real soon
- nrefactor
- reversibility
- last respoinsible momoent
- YAGNI
- what about extensibility
- parsimony
- triagulate
- Postel's law
- reuse
- minimyze library and framework
- sumaary

。 @venkat_s氏のジョーク交じり軽快なトークに耳がほとんどついていかず。スライドもなく、アジェンダとメモをスクリーンに表示してひたすらしゃべるしゃべるしゃべりまくるセッションだった。

耳はついて行けないものの、アジェンダが常に表示されており、どこまで話が進んでいるかも見た目で分かるようになっていた。そのため、なんとか話している方針や笑いどころを押さえることができるようになっており、彼の発表経験の豊富さを垣間見ることができた。

内容としては何か新しいものがあったわけではないが、アジャイルを実践しているからこそ言えるような「無駄をなくすためにどのように取組んでいるか」という観点での知見は非常に有用だったとおもう。

現場に来ないと価値がないセッションと言う意味では、わざわざUSAに来た甲斐があったものだったはずだったけど、このセッションはライブストリーミングでも見られたようでした。
