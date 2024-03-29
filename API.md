フロントでもAPI実装が必要になった背景  
マイクロサービスの流行/特定の機能ごとに小さく小さく開発していって、連結する時にAPIを使う手法。違う言語同士も連結できる  
便利なSaasの増加→API必須化  
フロントの役割増加  

## JAMスタック  
javascript  
API  
markdown  
を使った開発手法  
今の流行り  

## web概論
<webの用途は４つ>  
webサイト→情報の閲覧  
webアプリ→閲覧だけではなく、操作も可能。対話型  
UIとしてのweb  
web API→JSONがフォーマット  

<webを支える技術>  
HTTP：通信のためのルール＝プロトコル  
URI：リソースを識別するためのID（場所もファイルもわかる状態）。この中にURL（場所だけ分かっていて、ファイルの場所不明な状態）がある。  
HTML：Webで表示するための文書フォーマット  

<分散システムと集中システム>
馬鹿でかいコンピュータ（メインフレーム）があり処理を一手に引き受けているパターンが集中システム。webは世界規模の分散システム。  

## REST
設計思想（アーキテクチャスタイル）。設計モデルの１つ。この思想に基づいてAPIは作成する。
RESTによる実装例の１つがweb。  

<webにおけるリソース>  
web上に存在する名前を持ったあらゆる情報がリソース  
URIはリソースの名前＋住所　
複数のURIを持つリソースも存在する　

<RESTを構成する６つのアーキテクチャ>  
REST自体がアーキテクチャなのに、さらにそれを構成する要素となるアーキテクチャが存在する  

クライアント/サーバー：UIと処理を分離する仕組み  
ステートレスサーバー：クライアント状態をサーバーが管理しない仕組み。状態とは、クライアントが今どういう状態かをサーバー側は無視。どうでもいいとして気にしない。これがあるからリクエストの処理のみに集中できる。ステートフルは、cookieを使ってsessionを管理する場合。  
キャッシュ：サーバーから取得したリソースの再利用。通信回数が減るのでエコ。  
統一インターフェース：操作可能なインターフェースを限定。HTTPだったらGETとかPOSTとか８つしかない。このルールに従うこと自体を統一インターフェースと呼ぶ  
階層化システム：システムを階層に分離。いろんな種類のサーバーがある。ロードバランサー（アクセスを引き受け、どこのサーバーにわりてるかの処理の振り分けを行う）、プロキシサーバーとか  
コードオンデマンド：JSとかをクライアントでDL&実行するとき、実はサーバーに置いてあったJSのファイルをクライアントにDLして実行している。必要な時にコードを実行するよって意味  

## URI
URIはリソースを識別するID。世界で1個しかないものである必要がある。

https://torahack:pass@torahack.com:8000/posts?id=1&category=food#about

https　
URIスキーム：通信プロトコルの名称　

torahack:pass　
ユーザー情報　
username:password　
という形式。APIを使う時にユーザ情報を認証に利用する時に利用する。　

torahack.com　
ホスト名。DNSで名前解決できるドメイン名 or IPアドレス　

8000　
ポート番号。プロトコルで用いるTCPポート番号　

posts　
パス。階層や一意なリソースを表す　

id=1&category=food　
クエリパラメータ。名前＝値の形式で追加情報を付与する　

#about　
URIフラグメント。webページ内部の位置を指す　
ページ内スクロールとかで使う　

<URIのパスの指定方法>　
絶対URI　
先頭からのフルパス。長くなるのでめちゃくちゃめんどくさい。省略したいと考え相対URIが出来た。　　
https://github.com/Arranzt/md/edit/master/API.md　

相対URI　
起点からの相対的なパス　
github.com/Arranzt/md/edit/master/API.md　

ベースURI　
相対URIの起点を決めるためのURI　
https://　
htmlの<base>で作れる　

<URIと文字>　
URIではASCII文字が使える　
アルファベット　
数字　
記号　
日本語は%エンコーディングが必要。そのままでは使えないので、変換を挟んで使えるように変換している。　
ex　あ→%E3%81%B2　

<COOLなURIの設計方法>　
URIが変わらないということはcoolなこと。なんでかっていうと、URIはそもそもリソースへアクセスするための一意な情報。ずっとそのリソースへアクセスできるからCOOLと言われる。リンク切れが起こらない。　　
１　プログラミング言語に依存した情報を含めない　
　　ファイル拡張子、開発用ディレクトリ、大文字（原則小文字のみで作る！！）など　
  　http://a.com/index.phpはだめ。phpじゃなく別の言語にリファクタする時にphp指定されてたら終わる　
２　メソッド名やセッションIDを含めない　
　　/posts?action=hogeや/posts?sessionId=fuga　
  　セッションIDとかアクセスするたびに変わる　
３　URIにはリソースを指し示す名詞を使う　
　　つまり、動詞は使わないということ。動詞はHTTPメソッドで表現可能。　
４　なるべくシンプルかつ人間が読んで理解できる　
　　OK　http://torahack.com/posts/123　
   NG  http://torahack.com/posts/cat/food/article/123　
   こういうカテゴリの部分はクエリパラメータで利用できるように設計すべき　
   
<URIの変更方法>　
301 Redirect：恒久的なURIの変更を示す。このURIは、今後永久こっちのURIに変えるからね〜って意味。アクセスした時に５秒後に勝手にページが切り替わるあいつ。　
302 Redirect：一時的なURIの変更を示す　
原則、301 Redirectを使ってURIの変更を知らせる。そうするとページランクを引き継ぐことができ、SEOとしてつよつよになる　

<URI設計のテクニック>　
バージョン情報を含める　
http://www.google.com/v3　
拡張子を用いて多言語対応のリソースを切り替える　
http://torahack.com/portfolio.jp　
http://torahack.com/portfolio.en　

<URIの不透明性を保つ>　
URIからリソースの構造を推測できないようにすべき（＝ハッキングされる）　
URIに拡張子を含めると、その言語で操作される危険性を生む　
API経由以外でサーバーから情報を抜き出される危険性があるため、クライアント側でURIを構築しないこと  

## HTTPリクエストを投げる

<TCP/IPの4階層モデル>  
HTTPはTCP/IPというネットワークプロトコルがベースとなっている　
下から読む　

HTTP：アプリケーション層→アプリケーションごとの通信プロトコルを決定する層
TCP：トランスポート層→データ転送の抜け漏れをチェックする層。パケットが抜けてないかとかをチェックしている
IP：インターネット層→データを送るための層。IPアドレスとかパケット（データを小分けにしたもの）を制御している
Ethernet：ネットワークインターフェース層→LANケーブルなどの物理的な層  

<HTTPのバージョン>
1991 http0.9 GETメソッドのみ定義していた
1996 http1.0 レスポンスヘッダが追加され、ステータスコードがみれるように。POSTメソッドなどが追加された
1999 http1.1 原則、１通信＝１リクエスト＝１レスポンスとなるようにした
2015 http2.0 ストリームの多重化による複数リクエストの同時通信ができるようにした

ストリーム
仮想的なトンネルを作ることで、１つの通信で複数の情報のやり取りができるようになる技術

HTTPメッセージ

リクエストの場合
リクエストライン
ヘッダ
ボディ

レスポンスの場合
ステータスライン
ヘッダ
ボディ

HTTPはステートレスであれ
サーバーがクライアントの状態を管理せず通信すべし
ステートフルは簡潔だが、接続先が多くなると管理できなくなり死亡する
ステートレスは冗長でデータ量が多いが、シンプル
ステートレスで通信エラーが起きると重複が発生する可能性あり
 
ハンバーガショップの例
ステートフル

いらっしゃいませ、ご注文は何にされますか
ハンバーガをお願いします
ジュースは？
コーラで
＋50円でオプションつけれますがいかがですか
いらないです
かしこまりました
→従業員は回答を覚えている

ステートレス
いらっしゃいませ、ご注文は何にされますか
ハンバーガをお願いします
ジュースは？
ハンバーガをお願いします。コーラで
＋50円でオプションつけれますがいかがですか
ハンバーガをお願いします。コーラで。いらないです
かしこまりました
→従業員は回答を覚えていない

## HTTPメソッド

HTTPメソッドは８つだが、主要メソッドは６つ
４つのメソッドでCRUDを実装することが多い

GET　リソースの取得。CRUDのread
POST　リソースの追加、子リソースの作成など。CRUDのcreate
PUT　リソースの更新、なければ新規作成。CRUDのcreate/update
DELETE　リソースの削除。CRUDのdelete
HEAD　リソースのヘッダの取得
OPTION　リソースがサポートしているメソッドの取得

HTTPメソッドのべき等性と安全性
べき等性：ある操作を何回行っても結果が同じこと
安全性：操作対象のリソースの状態を変化させないこと
GET　べき等かつ安全
POST　べき等でも安全でもない
PUT　べき等だが安全ではない
DELETE　べき等だが安全ではない
HEAD　べき等かつ安全
OPTION　べき等かつ安全

メソッドの誤用
リソースの取得以外でGETを用いてしまう
他のメソッドをPOSTで代用してしまうと、
GETがべき等でも安全でもなくなる
PUTやDELETEがべき等でなくなる
という問題を生む
PUTで相対的な値を用いる
元の値に５０を足すというPUTメソッドを作ると、実行のたびに結果が変わることから、べき等性が消失する
なので、PUTを用いるのなら必ず絶対的な値を指定して利用すること。例えば、値を１００から１５０に変更する、みたいに

API実行まで
```
cluster:Desktop arranzt$ mkdir api_pra
cluster:Desktop arranzt$ cd api_pra
cluster:api_pra arranzt$ npm init

package name: (api_pra) 
version: (1.0.0) 
description: 
entry point: (index.js) 
test command: 
git repository: 
keywords: 
author: cluster
license: (ISC) 
About to write to /Users/arranzt/Desktop/api_pra/package.json:

{
  "name": "api_pra",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "cluster",
  "license": "ISC"
}

Is this OK? (yes) y

cluster:api_pra arranzt$ npm install --save express sqlite3 body-parser
cluster:api_pra arranzt$ npm install -g node-dev

package.json
"start": "node-dev app/app.js"を追加
{
  "name": "api_pra",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node-dev app/app.js"
  },
  "author": "cluster",
  "license": "ISC",
  "dependencies": {
    "body-parser": "^1.19.0",
    "express": "^4.17.1",
    "sqlite3": "^5.0.2"
  }
}

appディレクトリ追加
その中にapp.jsを追加

const express = require('express');
const app = express();

app.get('/api/v1/hello', (req, res) => {
    res.json({"message": "hello world"});
})

const port = process.env.PORT || 3000;
app.listen(port);
console.log("Listen on port" + port);

cluster:api_pra arranzt$ npm start

別ターミナル立ち上げ
cluster:api_pra arranzt$ curl -X GET "http://localhost:3000/api/v1/hello"
cluster:api_pra arranzt$ curl -X GET "http://localhost:3000/api/v1/hello" -v

```






| 名前 | 性別 | 出身地 |
|:-----------|------------:|:------------:|
| トイレの花子さん | 女性 | トイレ |
