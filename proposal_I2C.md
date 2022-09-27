# I2C

## 方針、リクエストなど

* マスターに特化して設計する。スレーブは当面考えない。
* 当面、7bitアドレスのみを考える。
* リピーテッドスタートを有効に使うことができるようにする。

--------------------------------------------------------------------------------
# コンストラクタ
```
I2C.new( *params )
```

### 例
```
i2c = I2C.new()
i2c = I2c.new( unit:1, frequency:400_000 )   # 400kHz
```

### オプションパラメータ

 | param | type | description |
 |-|-|-|
 | unit | --- | I2Cユニットの指定 |
 | frequency | Integer | 周波数 (デフォルト 100kHz) |


--------------------------------------------------------------------------------
# インスタンスメソッド
## 入力
```
read( i2c_adrs_7, read_bytes, *params ) -> String
```

* i2c_adrs_7 アドレスのデバイスから、read_bytes バイトのデータを読み込む。
* params にデータ（String or Integer) が指定されていれば、それを出力してからリピーテッドスタートを挟み読み込みを始める。
* 出力の仕様は、write() を参照。

### 戻り値

* 読み込んだバイト列がStringで返る。
* デバイスが途中で NAK を返す場合は、read_bytes より短い長さの String が返る可能性がある。

### 例
```
# 互換性のため、この形式は残したい。
i2c.read( 0x45, 3, 0xf3, 0x2d )

# 若干うざくないか？
i2c.read( address:0x45, write_data:[0xf3, 0x2d], read_bytes:3 )

# 混ぜることができると良いが、サポートするのは大変かもしれない。
i2c.read( 0x45, 3, write_data:[0xf3, 0x2d] )
```

I2C Sequence
```
(case 1) S -- adrs R A -- data_1 A -- ... -- data_n A|N -- P
(case 2) S -- adrs W A -- param_1 A -- ... -- param_n A --
         Sr -- adrs R A -- data_1 A -- ... -- data_n A|N -- P
    S : Start condition
    P : Stop condition
    Sr: Repeated start condition
    A : Ack
    N : Nack
    R : Read bit
    W : Write bit
```

 | param | type | description |
 |-|-|-|
 | stop | boolean | 最後にストップコンディションを出すか？ |
 <!--
 | read_bytes | Integer | 読み込みバイト数 |
 | write_data | Integer \| String \| Array[Integer] | 読み込み前に書き込むデータ |
-->


----------------------------------------
## 出力
```
write( i2c_adrs_7 , *params ) -> Integer
```

* i2c_adrs_7 アドレスのデバイスに、params で指定したデータを書き込む。

### 戻り値
* 書き込みできたバイト数が戻り値として返る。
  
### 例
```
i2c.write( 0x45, 0x30, 0xa2 )
i2c.write( 0x50, 0x00, 0x80, data_string )  # useful for EEPROM
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

 | param | type | description |
 |-|-|-|
 | stop | boolean | 最後にストップコンディションを出すか？ |


