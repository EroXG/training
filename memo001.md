 * [ryby-lang.org](https://docs.ruby-lang.org/ja/latest/doc/index.html)
原著 まつもとゆきひろ
より適宜引用

## 制御構造(Control Expessions)

>Rubyでは(Cなどとは異なり)制御構造は式であって、何らかの値を返すものが あります(返さないものもあります。値を返さない式を代入式の右辺に置くと syntax error になります)。 
* 「制御式」の方が内容を的確に表すか

### [if ](https://docs.ruby-lang.org/ja/latest/doc/spec=2fcontrol.html#if)(if Expression)

> また if の条件式が[正規表現のリテラル](https://docs.ruby-lang.org/ja/latest/doc/spec=2fliteral.html#regexp)である時には特別に
```
$_ =~ リテラル
```
> であるかのように評価されます。
* [$_](https://docs.ruby-lang.org/ja/latest/method/Kernel/v/_.html)


#### 偽と真

>Ruby では [false](https://docs.ruby-lang.org/ja/latest/class/FalseClass.html) または [nil](https://docs.ruby-lang.org/ja/latest/class/NilClass.html) だけが偽で、それ以外は 0 や空文 字列も含め全て真です。

<details><summary>false と nil 詳細</summary>

> false は FalseClass クラスの唯一のインスタンスです。 false は nil オブジェクトとともに偽を表し、 その他の全てのオブジェクトは真です。 [source](https://docs.ruby-lang.org/ja/latest/class/FalseClass.html)

> nil は NilClass クラスの唯一のインスタンスです。 nil は false オブジェクトとともに偽を表し、 その他の全てのオブジェクトは真です。[source](https://docs.ruby-lang.org/ja/latest/class/NilClass.html) 
</details>

* false, nil, true は[予約語](https://docs.ruby-lang.org/ja/latest/doc/spec=2flexical.html#reserved)

#### if 修飾子(Modifier if)

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

#### 条件演算子(Ternary if)

[参照](https://docs.ruby-lang.org/ja/latest/doc/spec=2foperator.html#cond)  

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


# markdown参考

* [guides.github](https://guides.github.com/features/mastering-markdown/)

* [emoji](https://github.com/ikatyang/emoji-cheat-sheet/blob/master/README.md)
