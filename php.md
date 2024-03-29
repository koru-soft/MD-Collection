## phpコマンド集

```
$ php phpファイル
```

## php文法まとめ

### phpの書き方
```
<?php

?>
```

### コメントアウト
```
//
#
/* */
```

### ヒアドキュメント
PHPのスクリプトエリアで、一時的にPHPとして解釈することを停止するための区域を指定する機能
終了検知文字列は予約語でなければ何でもOK。終了検知文字列の最後は、空白を前につけたらだめ。空白も終了検知文字列の一部として認識されるため。
```
<<< 終了検知文字列

終了検知文字列;


print <<< DOC_END

表示したいhtml要素など

DOC_END;
//左にぴったりくっつけること！
```

### 数値

| 進数 | 表記 | 例 |
|:-----------|------------:|:------------:|
| 10 | そのまま | 1 |
| 2 | 先頭に0bをつける | 0b1010 |
| 8 | 先頭に0をつける | 0123 |
| 16 | 先頭に0xをつける | 0x1A  |


### 最大・最小を求める関数

配列を引数に指定した場合、配列の要素内で最大・最小の値を検出する。  
変数群を引数に指定した場合、変数群の中で最大・最小の値を検出する。

```
max(検出したい変数群や配列)
min(検出したい変数群や配列)
```

### 四捨五入・切り上げ・切捨て 

四捨五入により指定した桁に変換した値を返す  
round  
```
round()
```
  
切り上げにより指定した値を返す  
ceil  
```
ceil()
```
  
切捨てにより指定した値を返す  
floor  
```
floor()
```

### 剰余
```
fmod(被除数, 除数)
```

### 指数
```
pow(基数, 指数)
```

### 乱数
```
rand();
rand(最小値, 最大値)
```

### 生成速度の速い乱数
```
mt_rand();
mt_rand(最小値, 最大値);
```

### 絶対値
引数が浮動小数点型の値なら浮動小数点型=float、それ以外は整数型の値を返す
```
abs(値);

abs(5);
→int 5

abs(5.2);
→float 5.2
```

### 
```

```

### 
```

```

### 
```

```

### 
```

```

### 
```

```

