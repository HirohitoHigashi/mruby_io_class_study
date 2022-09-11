# I2C

## コンストラクタ

### mruby/c 提案
```
i2c = I2C.new( machine dependent parameters )
```

### Ruby (gem)
i2c 0.4.2 https://rubygems.org/gems/i2c
```
i2c = I2C.create("/dev/i2c-1")
```

### MicroPython
```
i2c = I2C(freq=400000)          # ポートに依存して 400kHz の周波数でI2Cペリフェラルを作成します。
                                # 使用するペリフェラルやピンを選択するために追加のパラメータが必要になる場合があります
                                # I2C(id, *, scl, sda, freq=400000)
i2c = SoftI2C(scl, sda, *, freq=400000, timeout=50000)  # ソフトウェア I2C オブジェクトを新規に構築します。
```

### Arduino  
```
Wire.begin()
```

### 所感
I2Cマスタのみサポートを考えている。Arduinoだけ、スレーブ動作もできる。  
スレーブが必要になった場合は、専用の別なクラスを再設計した方がわかりやすいのではないか。ただ、マルチマスタをサポートするとなると、同じクラス内でサポートした方が良いと思う。どこまで考えておくか？  
複数のI2Cバスの利用方法についても、考えておくべき。  


--------------------------------------------------------------------------------
## 初期化

### mruby/c 提案
なし

### Ruby (gem)
なし

### MicroPython
```
i2c.init(scl, sda, *, freq=400000)
```

### Arduino
なし

### 所感
要るか？


--------------------------------------------------------------------------------
## 入力

### mruby/c 提案
```
read( i2c_adrs_7, read_bytes, *params ) -> String
```
Rubygem のライブラリと全く同じとしている。  

### Ruby (gem)
```
i2c.read(address, size, *params)
i2c.read_byte(address)
```
i2c_adrs_7 アドレスのデバイスから、read_bytes バイトのデータを読み込む。  
params が指定されていれば、まずそれを書き込んでから、リスタートを挟み、読み込みステートを開始する。  

### MicroPython
```
i2c.readfrom(42, 4)             # 7ビットアドレス 42 のペリフェラルから４バイトを読み込みます
i2c.readfrom_mem(42, 8, 3)      # スレーブ 42 のメモリから、ペリフェラルのメモリアドレス 8 で始まる３バイトを読み込みます
```

#### リファレンス
```
I2C.readfrom(addr, nbytes, stop=True, /)
I2C.readfrom_into(addr, buf, stop=True, /)
I2C.readfrom_mem(addr, memaddr, nbytes, *, addrsize=8)
I2C.readfrom_mem_into(addr, memaddr, buf, *, addrsize=8)
```

### Arduino
```
Wire.requestFrom(address, count)    # 他のデバイスにデータを要求します。
Wire.read()                         # 1バイト受信。複数バイト受信が必要ならループで対応する。
```

### 所感
1バイト読み込みと複数バイト読み込みのメソッドを分ける必要は、ほとんど無いだろう。  
Rubygem, mruby/c において、read と名前を付けているが、*params を指定した場合は、writeも行う。  
機能が名前を超えているような気もするが、I2Cバスにおいては標準的に現れる読み込みのためのバスオペレーションなので、別の名前をつけるまでもないかと感じる。ただ、引数の順番が、*params を出力後 read_bytes 分読み込むので、若干気に食わない。  



--------------------------------------------------------------------------------
## 出力

### mruby/c 提案
```
i2c.write( i2c_adrs_7, data1, data2,... )
i2c.write( i2c_adrs_7, "string" )
```

### Ruby (gem)
```
i2c.write(address, *params)
```

### MicroPython
```
i2c.writeto(42, b'123')         # 7ビットアドレス 42 のペリフェラルに３バイトを書き込みます
i2c.writeto_mem(42, 2, b'\x10') # スレーブ 42 のメモリの、ペリフェラルのメモリアドレス 2 で始まるところに２バイトを書き込みます
```

#### リファレンス
```
I2C.writeto(addr, buf, stop=True, /)
I2C.writevto(addr, vector, stop=True, /)
I2C.writeto_mem(addr, memaddr, buf, *, addrsize=8)
```
 

### Arduino
```
Wire.beginTransmission(address)     # 指定したアドレスのI2Cスレーブに対して送信処理を始めます。
Wire.write(value)                   # valueは、char, string, array が可能。
Wire.endTransmission()              # スレーブデバイスに対する送信を完了します。
```

### 所感
デバイスによっては、自分のアドレスが指定されたことで動作を開始し、規定の時間の後に読み込みをしなければならないものがあった。そういったデバイスをサポートするには、writeステートのあとストップコンディションを自動的に送らない機能を必要とする。  
MicroPythonでは、`I2C.writevto(addr, vector, stop=True, /)` となっており、ストップコンディションを制御できる。  
mruby/c でも、PSoC 版では 引数の最後に :NO_STOP を付けることで同等の動作を実現できていたが、あまり筋が良くない気がして提案書には盛り込んでいなかった。  


--------------------------------------------------------------------------------
## その他

### MicroPython
```
I2C.deinit()   # I2C バスをオフにします。
I2C.scan()    # 0x08 から 0x77 までのすべてのI2Cアドレスをスキャンし、応答したもののリストを返します。
```

#### プリミティブ I2C 操作（soft I2Cのみ）
```
I2C.start()
I2C.stop()
I2C.readinto(buf, nack=True, /)
I2C.write(buf)
```

### 所感
MicroPythonは、低レベルメソッド、標準バスオペレーション、メモリ操作と、バスのためのファンクションとその先につながるデバイスためのファンクションが混在し、機能過多ではないかと感じる。

