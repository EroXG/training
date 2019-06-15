
 * [ryby-lang.org](https://docs.ruby-lang.org/ja/latest/doc/index.html)
原著 まつもとゆきひろ
より適宜引用


##  [字句構造](https://docs.ruby-lang.org/ja/latest/doc/spec=2flexical.html) 



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

### [予約語](https://docs.ruby-lang.org/ja/latest/doc/spec=2flexical.html#reserved)

``` ruby
BEGIN    class    ensure   nil      self     when
END      def      false    not      super    while
alias    defined? for      or       then     yield
and      do       if       redo     true     __LINE__
begin    else     in       rescue   undef    __FILE__
break    elsif    module   retry    unless   __ENCODING__
case     end      next     return   until
```

<br>

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
* [$_](https://docs.ruby-lang.org/ja/latest/method/Kernel/v/_.html)


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

* ( )で括れば複文可能

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

* 制御が次の when 節に「落ちる」ことはないので break 不要。

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
break

break val
```

>  break はもっとも内側のループを脱出します。ループとは<br>  
    ・ while  
    ・ until  
    ・for  
    ・イテレータ<br><br>
のいずれかを指します。C 言語と異なり、break はループを脱出する作 用だけを持ち、case を抜ける作用は持ちません。  
break によりループを抜けた for やイテレータは nil を返します。 ただし、引数を指定した場合はループの戻り値はその引数になります

例(引数有りの場合)

``` ruby
check=
  0.upto(5).each do|i|
    break i*10 if i > 3
  end

p check #=>40
```

# [正規表現](https://docs.ruby-lang.org/ja/latest/doc/spec=2fregexp.html)(Regular Expression)

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

M.new #=> NoMethodError  Module クラスはインスタンスメソッド new を持たない
```

* 上記の意味において定義された各モジュール( M )はインスタンスを作成出来ない．しかし定義された各クラスが Class クラスのインスタンスであるのと同様、定義された各モジュールは Module クラスのインスタンスです．

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
  
  はModule クラスで定義されている。

### モジュール関数
#### module_function(*name) -> self

>  メソッドをモジュール関数にします。  
引数が与えられた時には、 引数で指定されたメソッドをモジュール関数にします。 引数なしのときは今後このモジュール定義文内で 新しく定義されるメソッドをすべてモジュール関数にします。  
モジュール関数とは、プライベートメソッドであると同時に モジュールの特異メソッドでもあるようなメソッドです。

> 関数のように用いられるメソッドの中で、 モジュールのメソッドとしても、特異メソッドとしても定義されて いるものはモジュール関数と呼ばれる。[souce](https://docs.ruby-lang.org/ja/latest/doc/glossary.html)

## [Encoding クラス](https://docs.ruby-lang.org/ja/latest/class/Encoding.html)

## [Symbol クラス](https://docs.ruby-lang.org/ja/latest/class/Symbol.html)

<br><br>

# markdown テスト
~~this~~

# markdown参考

* [guides.github](https://guides.github.com/features/mastering-markdown/)

* [emoji](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md)


