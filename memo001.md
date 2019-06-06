 * [ryby-lang.org](https://docs.ruby-lang.org/ja/latest/doc/index.html)
原著 まつもとゆきひろ
より適宜引用

## 制御構造(Control Expessions)
>Rubyでは(Cなどとは異なり)制御構造は式であって、何らかの値を返すものが あります(返さないものもあります。値を返さない式を代入式の右辺に置くと syntax error になります)。 

### [制御構造 if](https://docs.ruby-lang.org/ja/latest/doc/spec=2fcontrol.html#if)
#### 偽と真
>Ruby では [false](https://docs.ruby-lang.org/ja/latest/class/FalseClass.html) または [nil](https://docs.ruby-lang.org/ja/latest/class/NilClass.html) だけが偽で、それ以外は 0 や空文 字列も含め全て真です。

> false は FalseClass クラスの唯一のインスタンスです。 false は nil オブジェクトとともに偽を表し、 その他の全てのオブジェクトは真です。 [source](https://docs.ruby-lang.org/ja/latest/class/FalseClass.html)

>nil は NilClass クラスの唯一のインスタンスです。 nil は false オブジェクトとともに偽を表し、 その他の全てのオブジェクトは真です。[source](https://docs.ruby-lang.org/ja/latest/class/NilClass.html) 

* false, nil, true は[予約語](https://docs.ruby-lang.org/ja/latest/doc/spec=2flexical.html#reserved)

#### if 修飾子(modifier if)

例
 ```ruby
return if val == nil
```
>右辺の条件が成立する時に、左辺の式を評価してその結果を返します。 条件が成立しなければ nil を返します。
      







markdown参考
* [guides.github](https://guides.github.com/features/mastering-markdown/)