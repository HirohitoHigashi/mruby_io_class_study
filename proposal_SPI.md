# SPI

## 方針、リクエストなど

* この版では、簡単にするため8bit単位の転送のみを規定する。
* チップセレクト SS はGPIO機能を使う事とし、SPIクラスでは規定しない。

--------------------------------------------------------------------------------
# コンストラクタ
```
spi = SPI.new( *params )
```

### 例
```
spi = SPI.new()
spi = SPI.new( unit:1, frequency:1_000_000 )    # 1MHz
```

| param | type | desc. |
|-|-|-|
| unit | --- | SPIユニットの指定 |
| baudrate | Integer | ボーレート (default 1MHz) |
| mode | Integer | 0 to 3 (default 0) |
| first_bit | Constant | SPI::MSB \| SPI::LSB (default MSB) |

ハードウェアによって、全てがサポートされているとは限らない。

--------------------------------------------------------------------------------
# インスタンスメソッド
## 　指定された長さのデータを入力する
```
read( read_length ) -> String
```

* read_length 分のデータを読み込む。
* 同時に出力されるデータは、0が出力される。

### 例
```
data = spi.read( 32 )
```

----------------------------------------
## 指定されたデータを出力する
```
write( *params ) -> String
```

* params （String, Integer, Array\<Integer\>) 


### 例
```
spi.write( 0x30, 0xa2 )
spi.write( "\x30\xa2" )
```
