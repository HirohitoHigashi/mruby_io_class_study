# ADC

* アナログデジタル変換機能をサポートするクラス。
* 一般に、アナログ電圧値をデジタル値に変換する機能を持つ。


------------------------------------------------------------
## コンストラクタ
----------------------------------------
### ADC.new( pin, *params )

* pin で示す物理ピンを指定して、ADC オブジェクトを生成する。
* pin は標準的には整数で指定するが、別な方法（例えばPICでは"B1"等）があっても良い。
* ピンにアナログ・デジタルモードの切り替えがある機器の場合は、このタイミングでアナログモードに切り替える。
* 基本的に追加パラメータ params の指定は無いが、機種によってサンプリング速度などの追加機能指定がある場合は、ここへ指定する。

使用例
```ruby
adc1 = ADC.new( 1 )
```


------------------------------------------------------------
## インスタンスメソッド
----------------------------------------
### read_voltage() -> Float

* 値を読み込み、電圧値(V)を返す。

使用例
```ruby
v1 = adc1.read_voltage()
```

備考
* Floatが使用可能な状態でVMがコンパイルされている必要がある。
(MRBC_USE_FLOATマクロ。デフォルトON)
* 使用可能でない場合は、NotImplementedError例外が発生する。


----------------------------------------
### read() -> Float

* read_voltage のエイリアス


----------------------------------------
### read_raw() -> Integer

* 値を読み込み、raw値（電圧に変換前の値）を返す。

使用例
```ruby
v1 = adc1.read_raw()
```
