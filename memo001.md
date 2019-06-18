
 * [ryby-lang.org](https://docs.ruby-lang.org/ja/latest/doc/index.html)
原著 まつもとゆきひろ
より適宜引用

# 環境変数

* 注 ruby 内では global 扱い。よって先頭 $ は必要

$RUBYOPT

$RUBYPATH
> -S オプション指定時に、環境変数 PATH による Ruby スクリプトの探索に加えて、この環境変数で指定したディレクトリも 探索対象になります。

$RUBYLIB
> Rubyライブラリの探索パス$:のデフォル ト値の前にこの環境変数の値を付け足します。

$RUBYSHELL

$PATH

$RUBY_GC



# [疑似BNFによるRubyの文法](out.txt)

* sample/exyacc.rb による parse.y の処理結果 (out.txt)

# Ruby プログラムの実行

> Ruby プログラムの実行は文の連なりの評価です。なんらかの形であたえられたプログラムテキストをコンパイルし、BEGIN 文があればそれを評価し、トップレベルの式の連なりを評価し、END ブロックがあれば最後にそれを評価して終了します

トップレベル(top level) の定義

> クラス／モジュール定義の一番外側のコンテキスト。  Rubyスクリプトはトップレベルの コンテキストから処理が始まる。  
いきなり、 print "on Toplevel" というスクリプトを書いたとき、print メソッドはトップレベルから呼ばれている。  
トップレベルの self は main を指す。[ruby用語集](https://docs.ruby-lang.org/ja/latest/doc/glossary.html)

* Hello World プログラム

  ``` ruby
  print 'Hello World'
  ```

* トップレベルのself

  ``` ruby
   print self  #=> main
   ```
   ``` ruby
   print self.class  #=> mainObject
   ```

## [BEGIN](https://docs.ruby-lang.org/ja/latest/doc/spec=2fcontrol.html#BEGIN)

文法

``` ruby
BEGIN '{' 文.. '}'
```

>  初期化ルーチンを登録します。BEGINブロックで指定した文は当該ファ イルのどの文が実行されるより前に実行されます。複数のBEGINが指定 された場合には指定された順に実行されます。

>BEGINブロックはコンパイル時に登録されます。 BEGIN ブロックは、独立したローカル変数のスコープを導入しません。つまり、 BEGIN ブロック内で定義したローカル変数は BEGIN ブロックを抜けた後も使用 可能です。

>BEGINはトップレベル以外では書けません。全て SyntaxErrorになります。 

* 使用例 (require より前に実行される)

``` ruby
BEGIN {$LOAD_PATH << '/home/mylib'}
```
上記は -I オプション

``` ruby
ruby -I '/home/mylib'
```
と等価。


## 擬似変数
>  以下は、ローカル変数のように見えますが実際には予約語として処理され、 決まった値を返します。代入はできません。  
self  
記述されたブロックの self を返します。  
nil  
NilClass の唯一のインスタンス nil を返します。  
true  
TrueClass の唯一のインスタンス true を返します。  
false  
FalseClass の唯一のインスタンス false を返します。 

<br>

#  [字句構造](https://docs.ruby-lang.org/ja/latest/doc/spec=2flexical.html) 
> 改行は行が明らかに次の 行に継続する時だけ、空白文字として、それ以外では文の区切りと して解釈されます。 

例　空白文字として

``` ruby
a=
  100
puts a #=>100
```

``` ruby
def add a, b, c
  a+b+c
end

puts add(
  10,
  20,
  30
) #=>60
```

## [識別子](https://docs.ruby-lang.org/ja/latest/doc/spec=2flexical.html#identifier)(Identifier)

> Rubyの識別子は英文字またはアンダースコア('_')か ら始まり、英文字、アンダースコア('_')または数字 からなります。識別子の長さに制限はありません。


* つまり　/[a-zA-Z_]+[a-zA-Z0-9_]*/
* 識別子に該当しないもの  
empty?  
sub!


## [予約語](https://docs.ruby-lang.org/ja/latest/doc/spec=2flexical.html#reserved)

``` ruby
BEGIN    class    ensure   nil      self     when
END      def      false    not      super    while
alias    defined? for      or       then     yield
and      do       if       redo     true     __LINE__
begin    else     in       rescue   undef    __FILE__
break    elsif    module   retry    unless   __ENCODING__
case     end      next     return   until
```

* 分類案
  * end

  * 制御式 if then elsif else unless for in  while until do break next case when redo return

  * 論理 and or not

  * 擬似変数 self false nil true

  * 定義 def class module yield alias  undef super

  * 例外処理 begin rescue ensure  retry

  * defined?

  * BEGIN END

  * \_\_LINE__ \_\_FILE__ \_\_ENCODING__


* 予約語では無いもの　require include

<br>

# [プログラム・文・式](https://docs.ruby-lang.org/ja/latest/doc/spec=2fprogram.html)

## [式](https://docs.ruby-lang.org/ja/latest/doc/spec=2fprogram.html#exp)

>  Ruby の式には、変数と定数、さまざまなリテラル、それらの 演算子式、if や while などの制御構造、メソッド呼び出し(super・ブロック付き・yield)、 クラス／メソッドの定義があります。

> 式は括弧によってグルーピングすることができます。

> 空の式 () は nil を返します。

> Rubyの式には値を返す式と返さない式があります。 

* 「式」と「文」の言葉による細かい定義を詮索するのは不毛。必要あれば[疑似BNFによるRubyの文法 (ver は古い)](https://docs.ruby-lang.org/ja/2.2.0/doc/index.html) を参照。

# 制御構造(Control Expessions)

>Rubyでは(Cなどとは異なり)制御構造は式であって、何らかの値を返すものが あります(返さないものもあります。値を返さない式を代入式の右辺に置くと syntax error になります)。 

* 「制御式」の方が内容を的確に表すか

* 値を返す　if

## [if ](https://docs.ruby-lang.org/ja/latest/doc/spec=2fcontrol.html#if)(if Expression)

> また if の条件式が[正規表現のリテラル](https://docs.ruby-lang.org/ja/latest/doc/spec=2fliteral.html#regexp)である時には特別に

```
$_ =~ リテラル
```

> であるかのように評価されます。

[$_](https://docs.ruby-lang.org/ja/latest/method/Kernel/v/_.html) とは
> 最後に Kernel.#gets または Kernel.#readline で読み込んだ文字列です。 EOF に達した場合には、 nil になります。


### 偽と真

>Ruby では [false](https://docs.ruby-lang.org/ja/latest/class/FalseClass.html) または [nil](https://docs.ruby-lang.org/ja/latest/class/NilClass.html) だけが偽で、それ以外は 0 や空文 字列も含め全て真です。

<details><summary>false と nil 詳細</summary>

> false は FalseClass クラスの唯一のインスタンスです。 false は nil オブジェクトとともに偽を表し、 その他の全てのオブジェクトは真です。 [source](https://docs.ruby-lang.org/ja/latest/class/FalseClass.html)

> nil は NilClass クラスの唯一のインスタンスです。 nil は false オブジェクトとともに偽を表し、 その他の全てのオブジェクトは真です。[source](https://docs.ruby-lang.org/ja/latest/class/NilClass.html) 
</details>

* false, nil, true は[予約語](https://docs.ruby-lang.org/ja/latest/doc/spec=2flexical.html#reserved)

### if 修飾子(Modifier if)

例
 ```ruby
return if val == nil
```
```ruby
puts 'OK' if val == true
```
>右辺の条件が成立する時に、左辺の式を評価してその結果を返します。 条件が成立しなければ nil を返します。

* 右(左)辺はifの右(左)の式

:question: 条件が成立しなければ nil を返します。

``` ruby
val= true
left= 
  puts ""  if val == true

  p val
```

複文
``` ruby
(puts 'one'; puts 'two') if false
```

* ;で文を区切り、全体を( )で括れば複文可能

### [条件演算子](https://docs.ruby-lang.org/ja/latest/doc/spec=2foperator.html#cond)(Ternary if)
>式1 ? 式2 : 式3

は

>if 式1 then 式2 else 式3 end

と同じ


## case (case Expression)
文法

```
case [式]
[when 式 [, 式] ...[, `*' 式] [then]
  式..]..
[when `*' 式 [then]
  式..]..
[else
  式..]
end
```

* 制御が次の when 節に「落ちる」ことはないので break 不要。そもそも break は case から抜ける作用を持たない

>case は一つの式に対する一致判定による分岐を行います。when 節で指定された値と最初の式を評価した結果とを演算子 === を用いて 比較して、一致する場合には when 節の本体を評価します。 

>when 節の最後の式に `*' を前置すればその式は配列展開されます。 

>case の「式」を省略した場合、when の条件式が偽でない最初の 式を評価します。

例
``` ruby
case 
when arg.is_a? Interger
  val= arg.to_s
when arg.is_a? String
  val= arg
```

## 繰返し

### [break](https://docs.ruby-lang.org/ja/latest/doc/spec=2fcontrol.html#break)

文法
``` ruby
break[val]
```

>  break はもっとも内側のループを脱出します。ループとは<br>  
    ・ while  
    ・ until  
    ・for  
    ・イテレータ<br><br>
のいずれかを指します。C 言語と異なり、break はループを脱出する作 用だけを持ち、case を抜ける作用は持ちません。  
break によりループを抜けた for やイテレータは nil を返します。 ただし、引数を指定した場合はループの戻り値はその引数になります

例(引数指定の場合)

``` ruby
check=
  0.upto(5).each do|i|
    break i*10 if i > 3
  end

p check #=>40
```

<br>

# 例外処理

## raise
<br><br>




# メソッド呼出し(Calling Methods)

## 文法

```
[レシーバ  `.'] メソッド名 [`(' [[`*'] 式] ... [`&' 式] `)']
```

* ~~~[式 \`::'] メソッド名 [\`(' [[\`*'] 式] ... [\`&' 式] `)']~~~  __使うな!__

### レシーバ

* レシーバはオブジェクト

* レシーバ  `.'　が省略されるとレシーバは self になる(関数形式)

> メソッド呼び出しの際、private なメソッドは関数形式(レシーバを省 略した形式)でしか呼び出すことができません。また protected なメソッ ドはそのメソッドを持つオブジェクトのメソッド定義式内でなければ呼び出せ ません。



### メソッド名

メソッド名になり得るもの  
1. 識別子


1



## Safe Navigation Operator (&.)

>  メソッド呼び出しで \`.' の代わりに `&.' を使うことができます。 この形式でメソッドを呼びだそうとすると、レシーバが nil の場合は 以下のように働きます。
> * 引数の評価が行なわれない  
> * メソッド呼び出しが行われない
> * nil を返す

> レシーバが nil でない場合は通常のメソッド呼び出しが行われます。 
* NoMethodErrorが抑止できるかもしれないが、本来そのメソッド呼出しで得たかった結果は得られない。レシーバの nil チェックは行なうのが常道。


## 引数

> 引数の直前に * がついている場合、その引数の値が展開されて 渡されます。
展開はメソッド to_a を経由して行なわれます。

例
``` ruby
foo(1,*[2,3,4])          #=> foo(1,2,3,4)
foo(1,*[])               #=> foo(1)
foo(1,*[2,3,4],5)        #=> foo(1,2,3,4,5)
foo(1,*[2,3,4],5,*[6])   #=> foo(1,2,3,4,5,6)
```
> __最後__ の引数の直前に & がついている場合、その引数で指定した手続き オブジェクト(Proc)やメソッドオブジェクト(Method)がブロック としてメソッドに渡されます。

<br>

# クラス


# モジュール
モジュールの目的
> Modules serve two purposes in Ruby, namespacing and mix-in functionality. [source](https://docs.ruby-lang.org/en/trunk/syntax/modules_and_classes_rdoc.html)

* 名前空間とミックスインの機能

クラスとの相違
>  クラスとモジュールには  
    クラスはインスタンスを作成できるが、モジュールはできない。
    モジュールを他のモジュールやクラスにインクルードすることはできるが，クラスをインクルードすることはできない。

``` ruby
module M
  def initialize
  end
end

M.new #=> NoMethodError  Module クラスでインスタンスメソッド new は定義されていない
```

* 上記の意味において、定義された各モジュール( M )はインスタンスを作成出来ない．しかし定義された各クラスが Class クラスのインスタンスであるのと同様、定義された各モジュールは Module クラスのインスタンスです．

## [モジュール定義](https://docs.ruby-lang.org/ja/latest/doc/spec=2fdef.html#module)

文法1

``` ruby
 module 識別子
  式..
end
```

文法2

``` ruby
module 識別子
  式..
[rescue [error_type,..] [=> evar] [then]
  式..]..
[else
  式..]
[ensure
  式..]
end
```

> モジュールを定義します。モジュール名はアルファベットの大文字 で始まる識別子です。 rescue/ensure 節を指定し、例外処理ができます。

> モジュールが既に定義されいるとき、さらに同じモジュール名でモジュール定 義を書くとモジュールの定義の追加になります。 

## [Module クラス](https://docs.ruby-lang.org/ja/latest/class/Module.html)

* クラスの継承リスト: Module < Object < Kernel < BasicObject  
Class クラスの super class

* Module クラスで定義されたメソッドは super class として Class クラスに継承される。例えばアクセサメソッド  
  * attr_accessor
  * attr_reader
  * attr_writer   
は Module クラスで定義されている。

### モジュール関数
#### module_function(*name) -> self

>  メソッドをモジュール関数にします。  
引数が与えられた時には、 引数で指定されたメソッドをモジュール関数にします。 引数なしのときは今後このモジュール定義文内で 新しく定義されるメソッドをすべてモジュール関数にします。  
モジュール関数とは、プライベートメソッドであると同時に モジュールの特異メソッドでもあるようなメソッドです。

> 関数のように用いられるメソッドの中で、 モジュールのメソッドとしても、特異メソッドとしても定義されて いるものはモジュール関数と呼ばれる。[用語集](https://docs.ruby-lang.org/ja/latest/doc/glossary.html)


<br>

# [正規表現](https://docs.ruby-lang.org/ja/latest/doc/spec=2fregexp.html)(Regular Expression)

* 全てのマッチを取得するには String#scan を使う

## 特殊変数

>  パターンマッチしたときに、以下の特殊変数にマッチの情報をセットします。  

    $~ 最後にマッチしたときの情報(MatchData オブジェクト)
    $& マッチしたテキスト全体
    $` マッチしたテキストの手前の文字列
    $' マッチしたテキストの後ろの文字列
    $1, $2, ... キャプチャ文字列
    $+ 最後(末尾)のキャプチャ文字列

    これらの変数はスレッドローカルかつメソッドでローカルな変数です。 

## [正規表現リテラル](https://docs.ruby-lang.org/ja/latest/doc/spec=2fliteral.html#regexp)

> /で囲まれた文字列は正規表現です

* [%記法](https://docs.ruby-lang.org/ja/latest/doc/spec=2fliteral.html#percent)　%r!STRING! 


## [Regexp クラス](https://docs.ruby-lang.org/ja/latest/class/Regexp.html)

* self =~ string -> Integer | nil

> 文字列 string との正規表現マッチを行います。マッチした場合、 マッチした位置のインデックスを返します(先頭は0)。マッチしなかった 場合、あるいは string が nil の場合には nil を返 します。 

# [Encoding クラス](https://docs.ruby-lang.org/ja/latest/class/Encoding.html)

# [Symbol クラス](https://docs.ruby-lang.org/ja/latest/class/Symbol.html)

<br><br>
# temporary memo
$LOAD_PATH.unshift(File.dirname(__FILE__))

# markdown テスト
~~字消し~~
<hr>

__Bold__ plain



``` html
<br>
```

# markdown参考

* [guides.github](https://guides.github.com/features/mastering-markdown/)

* [emoji](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md)


