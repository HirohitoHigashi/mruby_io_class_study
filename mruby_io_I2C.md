# I2C

* I2C バスをサポートするクラス。
* この仕様書ではマスターデバイス、7ビットアドレスのみをサポートしている。

------------------------------------------------------------
## コンストラクタ
----------------------------------------
### I2C.new( id=nil, *params )

* id で示す物理ユニットを指定して、I2C オブジェクトを生成する。
* 物理ユニットが１つしか無い場合などは、idは省略可能とする。

オプションパラメータ

| param | type | description |
|-|-|-|
| frequency | Integer | 周波数 (デフォルト 100kHz) |
| freq | 同上 |
| scl_pin | --- | クロックピンの指定 |
| sda_pin | --- | データピンの指定 |

使用例
```ruby
# デフォルトの設定で、i2cオブジェクトを生成する。
i2c = I2C.new()

# ユニット1 の I2C デバイスを、周波数 400kHz で使う。
i2c = I2C.new(1, frequency:400_000 )   # 400kHz
```

機種依存
* 全てのパラメータがサポートされているとは限らない。


------------------------------------------------------------
## インスタンスメソッド
----------------------------------------
### read( i2c_adrs_7, read_bytes, *outputs ) -> String

* i2c_adrs_7 アドレスのデバイスから、read_bytes バイトのデータを読み込む。
* outputs が指定されていれば、それを出力してからリピーテッドスタートを挟み読み込みを始める。
* 出力の仕様は、write() を参照。
* デバイスが途中で NAK を返す場合は、read_bytes より短い長さの String が返る可能性がある。

オプションパラメータ

| param | type | description |
|-|-|-|
| stop | boolean | 最後にストップコンディションを出すか？ |

使用例
```ruby
s = i2c.read( 0x45, 3 )                 # case 1
s = i2c.read( 0x45, 3, 0xf3, 0x2d )     # case 2
s = i2c.read( 0x45, 3, "\xf3\x2d" )
```

I2C bus sequence
```
(case 1) S -- adrs R A -- data_1 A -- ... -- data_n A|N -- P
(case 2) S -- adrs W A -- out_1 A ... out_n A -- Sr -- adrs R A -- data_1 A -- ... data_n A|N -- P
    S : Start condition
    P : Stop condition
    Sr: Repeated start condition
    A : Ack
    N : Nack
    R : Read bit
    W : Write bit
```

----------------------------------------
### write( i2c_adrs_7 , *outputs ) -> Integer

* i2c_adrs_7 アドレスのデバイスに、outputs で指定したデータを書き込む。
* 書き込みできたバイト数が戻り値として返る。
* outputsは、Integer, Array\<Integer\> および String で指定する。

オプションパラメータ

| param | type | description |
|-|-|-|
| stop | boolean | 最後にストップコンディションを出すか？ |

使用例
```ruby
i2c.write( 0x45, 0x30, 0xa2 )
i2c.write( 0x50, 0x00, 0x80, data_string )  # useful for EEPROM
i2c.write( 0x11, [0x30, 0xa2] )
```

I2C Sequence
```
S -- adrs W A -- data_1 A -- ... -- data_n A|N -- P
    S : Start condition
    P : Stop condition
    A : Ack
    N : Nack
    W : Write bit
```
