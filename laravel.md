### ファイル構成

```
app
アプリケーションのプログラム部分がまとめられるところ。
ここに必要なファイルを追加していく。

bootstrap
アプリケーション実行時に最初に行われる処理がまとめられている

config
設定関係のファイルをまとめている

database
データベース関連のファイルをまとめている

public
公開フォルダ。JSやCSSなど、外部にそのまま公開されるファイルをまとめている

resources
リソース関連の配置場所。テンプレートファイルなどをまとめている

routes
ルート情報の保存場所。アクセスするアドレスに割り当てられるプログラムの情報などが記されている

storage
ファイルの保存場所。アプリケーションのプログラムが保存するファイルなどが配置される。
ログファイルもここに保管される

tests
ユニットテスト関係のファイルをまとめている

vendor
フレームワーク本体のプログラムをまとめている
```

### ルーティング

GETアクセス
```
Route::get(アドレス, 関数)

Route::get('/', function(){
  return view('welcome');
})
```

viewメソッド  
指定したテンプレートファイルをロードし、レンダリングして返す働きをしてくれる  
そのため、viewの引数にテンプレートを指定すると、それがレンダリングされて返され、ブラウザに表示される仕組みとなっている  
viewsフォルダからの検索となるので、パスの指定に注意
```
view(テンプレート名)

view('フォルダ名.ファイル名')

値をview側に渡したい時
view(テンプレート名, 配列)
```

ルートパラメータ  
アクセスする際にパラメータを設定し、値を渡したい時に使う
パラメータは必須パラメータと任意パラメータに分けられ、下記は必須パラメータの記述方法
```
Route::get('/アドレス/{パラメータ}', function(){});

Route::get('/welcome/{msg}', function(){
   return view('welcome');
})

この状態で/welcome/this_is_test
のようにURLを指定してリクエストを飛ばすと、URLにパラメータが付加される
```

任意パラメータ
パラメータの末尾に?をつけて宣言する方法  
パラメータを付けずともアクセスできるようにする仕組み
```
Route::get('/アドレス/{パラメータ?}', function(){});
```

### コントローラ

アクションとルートを割り当てる
```
Route::get(アドレス, コントローラ名@アクション名)
```

アクションとアドレスの関係
```
http://アプリケーションのアドレス/コントローラ/アクション
```

シングルアクションコントローラ
```
class コントローラ extends Controller
{
  public function __invoke(){
    アクション処理
  }
}

ルーティングの設定方法
Route::get(アドレス, コントローラ名)
```

### リクエストとレスポンス

RequestクラスとResponseクラスを用いる
```
use Illuminate\Http\Request
use Illuminate\Http\Response

使い方
public function index(Request $request, Response $response){処理}
```

### Requestクラスで用意されている主なメソッド  

アクセスしたURLを返す（クエリ文字を除く）
```
$request->url();
```

アクセスしたURLを返す（クエリ文字を含む）
```
$request->fullurl();
```

ドメインのパス部分だけを返す
```
$request->path();
```

### Responseクラスで用意されている主なメソッド

アクセスに関するステータスコードを返す
```
$this->status();
```

コンテンツの取得を行う
```
$this->content();
```

引数の値にコンテンツを変更する
```
$this->setContent(値);
```

### ビュー

値をviewに渡す
```
値をview側に渡したい時
view(テンプレート名, 配列)

$data = ['msg' => 'テストメッセージ'];
return view('hello.index', $data);
```

アクセスするアドレスの情報を利用して値を渡す場合  
クエリ文字列を使う
```
public function index(Request $request){
  $data = [
    'msg' => 'テストメッセージ',
    'id' => $request->id
  ]
  return view('hello.index', $data);
}

Route::get('hello', HelloController@index);

?id=メッセージとつけてアクセスすると、その内容が$request->キー名で取得できるようになる
http://localhost:8000/hello?id=fjrgjoagrigag
```

### フォーム送信

フォームの作成
```
<form method="POST" action="/hello">
  @csrf
  <input type="text" name="msg">
  <input type="submit">
</form>
```

アクションの用意
```
public function post(Request $request)
{
  $msg = $request->msg;
  $data = [
    'msg' = 'こんにちは' . $msg . 'さん',
  ];
  return view('hello.index', $data);
}
```

ルーティングの用意
```
Route::post('hello', 'HelloController@post');
```

### Bladeで利用するディレクティブ群

値の表示  
{{}}の間に文を書くことで、その文が返す値をその場に書き出す  
HTMLのエスケープ処理が自動的に行われる  
そのため、HTMLタグなどを書いてもテキストとしてのタグとして処理され、HTMLタグとして処理されない  
エスケープ処理をさせたくない場合は、{!! !!}の間に文を書く
```
{{ 値・変数・式・関数 }}

{!! 値・変数・式・関数 !!}
```

@ifディレクティブ
```
@if(条件)

@elseif

@else

@endif
```

@unlessディレクティブ
条件が非成立の場合に、処理を動かす
```
@unless(条件)

@endunless
```

@emptyディレクティブ
変数が空の場合に処理を動かす
```
@empty(条件)

@endempty
```

@issetディレクティブ
変数そのものが定義されている場合、処理を動かす
```
@isset

@endisset
```

@forディレクティブ
繰り返し処理を行う
```
@for(初期化;　条件;　後処理)

@endfor
```

@foreachディレクティブ
配列から値を取り出して処理をする
```
@foreach($配列 as $変数)

@endforeach
```

@forelseディレクティブ
配列から順に値を取り出し処理を繰り返すが、値を全て取り出し終えた時、@emptyにある処理を実行して繰り返しを終える
```
@forelse($配列 as $変数)

@empty

@endforelse
```

@whileディレクティブ
```
@while(条件)

@endWhile
```

@breakディレクティブ  
PHPのbreakに相当。これが出力されると、その時点で繰り返しのディレクティブが中断される  

@continueディレクティブ  
PHPのcontinueに相当。これより後は表示せず、すぐに次の繰り返しに進む  

### ループ変数
繰り返しディレクティブには$loopという特別な変数が用意されており、繰り返しに関する情報を保持してくれている
```
$loop->index
現在のインデックスを取り出す

$loop->iteration
現在の繰り返し数を取り出す

$loop->remaining
後何回繰り返すかという残り回数を取りだす

$loop->count
繰り返しで使っている配列の要素数を取り出す

$loop->first
最初の繰り返しかどうかを判定する

$loop->last
最後の繰り返しかどうかを判定する

$loop->depth
繰り返しのネスト数を判定する

$loop->parent
ネストしている場合、親の繰り返しのループ変数を示す
```

### @PHPディレクティブ

PHPのスクリプトを直接実行したい時に利用する
```
@PHP

@endphp
```

### Bladeの文法

継承  
すでにあるテンプレートのレイアウトを継承して新しいテンプレートを作成する  

セクション  
継承でページをデザインする時に、ページ内の要素として活用される  

@section  
ページに表示されるコンテンツの区画を定義する  
このセクションは、@yieldに嵌め込まれ、表示される
```
@section(名前)

@endsection
```

@yield  
セクションの内容をはめ込んで表示するためのディレクティブ  
@yieldは配置場所を示すもの
```
@yield(名前)
```

@extends  
レイアウトの継承設定を行
```
@extends('親レイアウトのbladeファイル名')

@extends('layouts.helloapp')
layoutsフォルダ内のhelloapp.blade.phpというレイアウトファイルをロードし、親レイアウトとして継承しますという意味になる
```

@parent  
親レイアウトのセクションを示す  

@component
```
@component(名前)

@endcomponent
```

### @slot
{{}}で指定された変数に値を設定するための仕組みをスロットと呼ぶ
```
@slot(名前)

@endslot
```

### サブビュー
あるビューから別のビューを読み込み、はめ込んだものをサブビューと呼ぶ
```
@include('テンプレート名', [値の指定])
```

### コレクションビュー
予め用意されていた配列やコレクションから順に値を取り出し、指定のテンプレートにはめ込んで出力するものをコレクションビューと呼ぶ
```
@each(テンプレート名, 配列, 変数名)
```

### サービス
Laravelに用意されている機能強化のための仕組みの１つ。Laravelにはサービスコンテナというシステムが用意されており、DIによって必要に応じてサービスと呼ばれるプログラムを自動的に自身の中に組み込み、使えるようにしてくれる。このサービスとサービスコンテナにより、必要に応じて各種の機能をアプリケーション内に組み込み、機能拡張していくことができるようになっている。

### サービスプロバイダ
サービスを登録しておくためのシステム。予め登録しておくための設定ファイルが用意されており、そこに記述するだけでアプリケーションに組み込まれるようになる。
```
<?php

namespace App\Providers;
use Illuminate\Support\ServiceProvider;

class プロバイダクラス名 extends ServiceProvider
{
    public function register()
    {
        
    }
    registerメソッドは、サービスプロバイダの登録処理を行なってくれる。サービスを登録する処理などはここに記述する。
    
    public function boot()
    {
        
    }
    bootメソッドはアプリケーションへのブートストラップ処理（アプリケーションが起動する際に割り込んで実行される処理）を登録する箇所。
}
```

### ビューコンポーザ
コントローラでもモデルでもなく、ビューのみで利用するロジックを、ビューに直接書かず別の箇所に記載するための機能。これがあるおかげで、複雑なロジックをビューに直接書かなくても済んでいる
```
View::composer(ビューコンポーザが割り当てるビューの指定, 実行する処理となるクロージャかビューコンポーザのクラス)

class HelloServiceProvider extends ServiceProvider
{   
    public function boot()
    {
      View::composer(
        'hello.index', function($view){
          $view->with('view_message', 'composer message');
        }
      );   
    } 
}
```
viewsフォルダ内のhelloフォルダ内にあるindex.blade.phpにview_messageという値を設定して処理している  
$viewにはIlluminate/Viewクラスのインスタンスが自動的に入る  
Illuminate/ViewクラスはViewを管理するオブジェクトとなっており、ここにあるメソッドを利用することでビューを操作できるようになる  
  
withメソッド  
ビューに変数などを追加するメソッド
```
$view->with(変数名, 値)
```

サービスプロバイダの登録
config/app.phpに記述する  
providersという名前に設定された配列が、アプリケーションに登録されているプロバイダの一覧になる。ここにプロバイダクラスを追加することで、アプリケーション起動時にそれが登録され、利用できるようになる。
```
'providers' => [

        /*
         * Laravel Framework Service Providers...
         */
        Illuminate\Auth\AuthServiceProvider::class,

        /*
         * Package Service Providers...
         */

        /*
         * Application Service Providers...
         */
        App\Providers\AppServiceProvider::class,
        App\Providers\HelloServiceProvider::class

    ],

```

### ミドルウェア
リクエストを受け取るとコントローラ処理の前後に割り込み、独自の処理を追加する仕組み  
「全てのアクセス時に何かを処理しておく」という記載をしたい時に役立つ  
例えば、フォームのページを1億用意しておき、バリデーションさせる場合、フォームのページごとにバリデーションを実装すると日が暮れるので、「指定のアドレスにリクエストが送られてきたら、自動的になんらかの処理を行う」という仕組みがある  
app/http/middlewareにある
```
<?php

namespace App\Http\Middleware;

use Closure;

class HelloMiddleware
{
    public function handle(Request $request, Closure $next)
    {
        return $next($request);
    }
}

```
$nextについて、これはClosureクラスのインスタンスになっている。これは無名クラスを表すためのクラス。ここで渡された$nextはクロージャとなっており、これを呼び出して実行することでミドルウェアからアプリケーションへと送られるリクエスト(Requestインスタンス)を作成することができる。  
  
mergeメソッド  
フォームの送信などで送られるinputの値に、新たな値を追加するもの
```
$request->merge(配列)

class HelloMiddleware
{
  public function handle(Request $request, Closure $next)
    {
        $data = ['name' => 'taro'];
        $request->merge(['data' => $data]);
        return $next($request);
    }
}
```

ミドルウェアの呼び出し
```
use App\Http\Middleware\HelloMiddleware;

Route::get('hello', 'HelloController@index')->middleware(HelloMiddleware::class);
```

Afterミドルウェア
```
public function handle(Request $request, Closure $next)
    {
        $response = $next($request)
          処理
        return $response;
    }
```

### グローバルミドルウェア
全てのアクセスで自動的にミドルウェアを実行されるようにしたい、という場合に使うミドルウェア  
全てのリクエストで利用可能となる  
app/httpの中のKernel.phpの$middlewareに記述することで使えるようになる。
```
class Kernel extends HttpKernel
{
    protected $middleware = [
        \App\Http\Middleware\HelloMiddeleware::class,
        →こんな風に登録する
        
        \App\Http\Middleware\TrustProxies::class,
        \Fruitcake\Cors\HandleCors::class,
        \App\Http\Middleware\PreventRequestsDuringMaintenance::class,
        \Illuminate\Foundation\Http\Middleware\ValidatePostSize::class,
        \App\Http\Middleware\TrimStrings::class,
        \Illuminate\Foundation\Http\Middleware\ConvertEmptyStringsToNull::class,
    ];
}
```
呼び出す時、middlewareのメソッドチェーンも不要になる
```
X Route::get('hello', 'HelloController@index')->middleware(HelloMiddleware::class);

○ Route::get('hello', 'HelloController@index');
```

ミドルウェアのグループ化
kernel.phpの$middlewareGroupsを利用する
```
protected $middlewareGroups = [
        'web' => [
            ミドルウェアのクラス
        ],

        'api' => [
           ミドルウェアのクラス
        ],
    ];
```
このwebやapiという部分がグループになる  
グループ名をキーとする値を用意し、そこにミドルウェアの配列を設定する

### バリデーション

ValidateRequestsというトレイトにvalidateメソッドが定義されており、これを使うことでバリデーションを実装できる
```
$this->validate($request, [検証設定の配列]);
```

バリデーションの例
```
public function post(Request $request){
  $validate_rule = [
    'name' => 'required',
    'mail' => 'email',
    'age' => 'numeric|between:0,150',
  ];
  $this->validate($request, $validate_rule);
  return view('hello.index', ['msg' => '正しい入力です']);
}
```

### $errors
バリデーションで発生したエラーメッセージをまとめて管理してくれるオブジェクト。  
バリデーション機能を利用すると勝手に組み込まれる。  
  
hasメソッド  
エラーが発生しているかを調べるメソッド
```
$errors->has(項目名)

@if($errors->has('name'))
{{ $errors->first('name') }}
@endif
```

firstメソッド  
指定した項目の最初のエラーメッセージを取得するためのメソッド
```
$errors->first(項目名)
```

getメソッド  
エラーは１つしかないわけではない。複数のエラーが発生した場合、当然エラーメッセージも複数送られてくる。こんな時、firstでは１つしか取れない。そこで、指定した複数項目のメッセージを取得できるgetメソッドを利用する。
```
$errors->get(項目名)
```

allメソッド  
全てのエラーメッセージを取得する
```
$errors->all()
```

@errorディレクティブ  
（）に指定した名前の項目のエラーをチェックし、その名前の入力項目でエラーが発生していたなら、このディレクティブ内に記述した内容を表示する。このディレクティブ内では、発生したエラーメッセージが$messagesという変数として渡されてくる。
```
@error(名前)
  $messageでメッセージを表示
@enderror

@error('name')
{{ $message }}
@enderror
```

### バリデーションの検証ルール

その項目がtrue、on、yes、1といった値かどうか＝チェックボックスにチェックが入っているかをチェックする
```
accept
```

active_urlはアドレスで指定されたドメインが実際に有効なものかどうかをチェックする  
（dns_get_record関数でDNS情報を取得し、有効なIPアドレスかどうかをチェックしている）  
urlは単にurlの形式で書かれているかどうかをチェックする
```
active_url
url
```

afterは指定した日付よりも後の日付が設定されているかどうかをチェックする  
after_or_equalは、指定した日付と同じかそれより後であるかをチェックする
```
after:日付
after_or_equal:日付
```

beforeは指定した日付より前かどうかをチェックする  
before_or_equalでは指定の日付と同じか、それより前かどうかをチェックする
```
before:日付
before_or_equal:日付
```

alphaは入力したテキストが全てアルファベットであるかをチェックする  
alpha-dashは入力したテキストがアルファベットかハイフンかアンダースコアかどうかをチェックする  
alpha-numは入力したテキストがアルファベットか数字かをチェックする
```
alpha
alpha-dash
alpha-num
```

フィールドが配列となっているかどうかをチェックする
```
array
```

複数のバリデーションルールが２つの項目に適用されている時、バリデーションエラーが発生したら残りのバリデーションルールの適用を中断する
```
bail
```

値が指定の範囲内かどうかをチェックする
```
between:最小値, 最大値
```

値がbooleanかをチェックする（true/false/0/1の４つを判定する）
```
boolean
```

パスワードなど、同じ値を二重に入力する場合のチェックを行う
```
confirmed
```

dateは、入力されたテキストが日時の値として扱えるものかをチェックする（＝strtotime関数でタイムスタンプに変換できるかが判定基準）  
date_equalsは、入力された値が指定の日付と同じ日付かどうかをチェックする  
date_formatは、入力された値が指定フォーマットの定義に一致しているかどうかをチェックする
```
date
date_equals:日付
date_format:フォーマット
```

differentは指定されたフィールドと異なる値となっているかをチェックする  
sameは指定されたフィールドと同じ値となっているかをチェックする
```
different:フィールド
same:フィールド
```

digitsは指定の桁数ならOK  
digits_betweenは最小桁数〜最大桁数の範囲内であればOKとする
```
digits:桁数
digits_between:最小桁数,最大桁数
```

イメージファイルなどを設定する場合に用いる。対象ファイルの内容が設定内容に合致するかどうかを示す。
```
dimensions:設定内容

設定内容
min_width, max_width　最小幅、最大幅
min_height, max_height　最小高さ、最大高さ
width, height　幅、高さ
_ratio_　縦横比

幅が50以上、500以下と設定する場合
dimensions: min_width=50, max_width=500
```

配列内に同じ値がないかチェックする
```
distinct
```

電子メールアドレスの形式かどうかをチェックする
```
email
```

データベースを利用する際、入力された値が、指定されたデータベースの指定のカラムにあるかどうかをチェックする
```
exists:　テーブル、カラム
```

値がアップロードに成功したファイルかどうかをチェックする
```
file
```

filledはその項目が空でないかをチェックする  
requiredは必須項目であることを示す
```
filled
required
```

指定されたファイルがイメージファイルかどうかをチェックする
```
image

gt → >
gte → >=
lt → <
lte → <=
```

### バリデーションのカスタマイズ
FormRequestを利用する
```

```

### DBクラス(クエリビルダ)
DBクラスを利用して、データベース処理を行う方法。DBクラスはデータベースアクセスのための基本的な機能をまとめたクラスで、ここにあるメソッドを呼びだすことでデータベースにアクセスできるようになる
```
DB::メソッド

DB::select('select * from users')
```

### Eloquent(ORM)
ORMという機能をLaravelに実装したもの  

### データベースの設定
config/database.phpに設定がある
```
<?php

use Illuminate\Support\Str;

return [

    'default' => env('DB_CONNECTION', 'mysql'),
    env関数→Laravelの環境変数を設定してくれる変数

    'connections' => [

        'sqlite' => [
            'driver' => 'sqlite',
            ドライバー名
            
            'url' => env('DATABASE_URL'),
            'database' => env('DB_DATABASE', database_path('database.sqlite')),
            使用するデータベース名をDB_DATABASEという環境変数に設定しておく
            database_path関数はlaravelのdatabaseフォルダ内のパスを返り値として返す関数
            
            'prefix' => '',
            データベースの名前の前に付ける文字列の指定
            
            'foreign_key_constraints' => env('DB_FOREIGN_KEYS', true),
        ],

        'mysql' => [
            'driver' => 'mysql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '3306'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'unix_socket' => env('DB_SOCKET', ''),
            'charset' => 'utf8mb4',
            'collation' => 'utf8mb4_unicode_ci',
            'prefix' => '',
            'prefix_indexes' => true,
            'strict' => true,
            'engine' => null,
            'options' => extension_loaded('pdo_mysql') ? array_filter([
                PDO::MYSQL_ATTR_SSL_CA => env('MYSQL_ATTR_SSL_CA'),
            ]) : [],
        ],

        'pgsql' => [
            'driver' => 'pgsql',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', '127.0.0.1'),
            'port' => env('DB_PORT', '5432'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'charset' => 'utf8',
            'prefix' => '',
            'prefix_indexes' => true,
            'schema' => 'public',
            'sslmode' => 'prefer',
        ],

        'sqlsrv' => [
            'driver' => 'sqlsrv',
            'url' => env('DATABASE_URL'),
            'host' => env('DB_HOST', 'localhost'),
            'port' => env('DB_PORT', '1433'),
            'database' => env('DB_DATABASE', 'forge'),
            'username' => env('DB_USERNAME', 'forge'),
            'password' => env('DB_PASSWORD', ''),
            'charset' => 'utf8',
            'prefix' => '',
            'prefix_indexes' => true,
        ],
        ],
    ],
];

```

### 環境変数
.envファイルに記述するものが環境変数と呼ばれる  
Laravelの基本的な動作環境に関する変数群のこと

### DBクラスの操作
```
レコードの取得
DB::select(実行したいSQL文)

レコードの追加
DB::insert(クエリ文, パラメータ配列)

レコードの更新
DB::update(クエリ文, パラメータ配列)

レコードの削除
DB::delete(クエリ文, パラメータ配列)
```

### プレースホルダ
:名前という形で値を確保しておく場所を確保する方法をプレースホルダと呼ぶ。その後の引数にある配列から値を指定の場所にはめ込んでSQL文を完成させるのに用いる

### クエリビルダ
SQLのクエリ文を生成するために用意された一連のメソッド群のことをクエリビルダと呼ぶ。  
Illuminate/Database/QueryにあるBuilderクラスがSQL文生成の機能を定義してくれている
```
DB::table(テーブル名)
```

テーブルのレコードを取得する  
実行結果は配列として返ってくる
```
DB::table(テーブル名)->get();
※カラムを指定する場合にはget(カラム名)とする

DB::table('users')->get();
DB::table('users')->get(['id', 'name']);
```

テーブルからレコードを取得する際の条件を指定する
```
where(フィールド名, 値)

DB::table('users')->where('id', 1)->get();
```

最初のレコードのみ取得する
```
first()

DB::table('users')->where('id', 1)->first();
```






