# SPI

## コンストラクタ

### mruby/c 提案
```
spi = SPI.new( machine dependent parameters )
```

### Ruby (spi)
spi 0.1.1  https://rubygems.org/gems/spi
```
spi = SPI.new(device: '/dev/spidev32766.0')
```

### Ruby (fubuki)
fubuki 1.0.1 https://rubygems.org/gems/fubuki
```
spi_bus = 0
spi_device = 0
spi = Fubuki::SPI.new(spi_bus, spi_device)
```

### MicroPython
```
spi = SPI(0, baudrate=400000)           # 周波数 400kHz で SPI ペリフェラル 0 を作成
                                        # ユースケースによっては、追加のパラメータが必要な場合があります。使用するバスの
                                        # 特性やピンを選択するための追加のパラメータが必要になる場合があります。
```

### Arduino  
```
SPI.begin()
```

### 所感


--------------------------------------------------------------------------------
## 初期化

### mruby/c 提案
なし

### Ruby (spi)
```
s.speed=500000
```

### MicroPython
```
init(baudrate=1000000, *, polarity=0, phase=0, bits=8, firstbit=SPI.MSB, sck=None, mosi=None, miso=None, pins=(SCK, MOSI, MISO))
```

### Arduino
```
SPI.setBitOrder(order)
SPI.setClockDivider(SPI_CLOCK_DIV2)
SPI.setDataMode(SPI_MODE0)

SPI.beginTransaction(SPISettings(14000000, MSBFIRST, SPI_MODE0))
```

### 所感

Rubygem版は、スピードの設定のみ存在するようだ。  
MicroPython の用語 baudrate は、SPIに使うには少々奇妙だと思ったが、フリースケールセミコンダクタのSPI規格書に使われている正式な単語であった。  


--------------------------------------------------------------------------------
## 入力

### mruby/c 提案
```
read( read_bytes ) -> String
```

### MicroPython
```
read(nbytes, write=0x00)    # write で指定した１バイトを連続で書き込みながら、 nbytes で指定したバイト数を読み込みます。
                            # 読み込んだデータを持つ bytes 型オブジェクトを返します。
readinto(buf, write=0x00)   # write で指定した１バイトを連続で書き込みながら、 buf に指定したバッファに読み込みます。
```

### 所感
Rubygem, Aruduino版は、汎用関数のみしか無いので、そちらへ記載。


--------------------------------------------------------------------------------
## 出力

### mruby/c 提案
```
write( data1, data2,... )
write( "string" )
```

### MicroPython
```
write(buf)     # buf に含まれる bytes 型オブジェクトを書き込みます。 None を返します。
```

### 所感
Rubygem, Aruduino版は、汎用関数のみしか無いので、そちらへ記載。


--------------------------------------------------------------------------------
## 汎用転送

### mruby/c 提案
```
transfer( [d1, d2,...], recv_size ) -> String
```

### Ruby (spi)
```
xfer(txdata: [], length: 0)
```

### Ruby (fubuki)
```
spi_speed = 1_000_000
spi_delay = 10 # usec
response = spi.transfer([0x01, 0x02, 0x03], spi_speed, spi_delay)
```

### MicroPython
```
write_readinto(write_buf, read_buf)    # read_buf に読込みながら write_buf の bytes 型オブジェクトを書き込みます。
                                       # 両バッファは同じでも異なっていてもかまいませんが、同じ長さでなければなりません。 
                                       # None を返します。
```

### Arduino
```
receivedVal = SPI.transfer(val)
receivedVal16 = SPI.transfer16(val16)
SPI.transfer(buffer, size)
```

### 所感
SPIバスの動作(FIFO)を最も良く表す機能だが、メソッド名に悩む。Arduinoの `transfer` が良いように思うが３種類も必要ない。  

