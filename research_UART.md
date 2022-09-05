# UART
--------------------------------------------------------------------------------
## コンストラクタ

### mruby/c 提案
```
UART.new( machine dependent parameters )
uart1 = UART.new( 1 )	# param: grove pin number.
```

### Ruby (gem)
serialport 1.3.2 https://rubygems.org/gems/serialport/
```
SerialPort.new("port", *params)
```

### MicroPython
```
uart = UART(1, 9600)                         # 与えたボーレートで初期化
```

### Arduino
```
Serial.begin(speed)
```

### 所感


--------------------------------------------------------------------------------
## 設定

### mruby/c 提案
```
#set_modem_params(hash)
```

### Ruby (gem)
```
#set_modem_params(baud, data_bits, stop_bits, parity)
#set_modem_params(hash)
```

### MicroPython
```
uart.init(9600, bits=8, parity=None, stop=1)
```

### Arduino
```
なし
```

### 所感
mruby/c 提案は、Rubygemからもらってきたもの。  
GPIOと同じようにコンストラクタで全て指定してしまえば良いが、UARTは通信途中でパラメータ（ボーレート）を変更するケースがあるので、設定用メソッドがあった方が良い。


--------------------------------------------------------------------------------
## 入力

### mruby/c 提案
```
# 指定されたバイト数のデータを読み込みます。指定されたバイト数のデータが到着していない場合、nilを返します。
val = uart1.read( 10 )  

# 指定されたバイト数のデータを読み込みます。指定されたバイト数のデータが到着していない場合、到着している分のデータを返します。
val = uart1.read_nonblock( 1024 ) 

# 文字列を一行読み込みます。実際には受信キュー内の "\n" までのバイト列を返します。 受信キューに "\n" が無い場合、nilを返します。
val = uart1.gets()
```

### Ruby (gem)
IOクラスを継承しているので、IOクラスのメソッド群を使う。下記は一例。
```
val = uart1.read( 10 )
```

### MicroPython
```
uart.read(10)       # 10文字を読み込んで、bytes 型オブジェクトを返す
uart.read()         # 可能な限り文字を読み込む
uart.readline()     # 1行を読み込む
uart.readinto(buf)  # 読み込んで、与えたバッファに格納
```

### Arduino
```
# 読み込み可能なデータの最初の1バイトを返します。-1の場合は、データが存在しません。
Serial.read()

# reads characters from the serial port into a buffer. 
Serial.readBytes(buffer, length)

# reads characters from the serial buffer into an array.
Serial.readBytesUntil(character, buffer, length)

# reads characters from the serial buffer into a String.
Serial.readString()

# reads characters from the serial buffer into a String.
Serial.readStringUntil(terminator)
```

### 所感

POSIX APIなど、データが到着していなければブロックする方式が一般的ですが、mruby/cで作るアプリケーションの場合、あえてブロックしない方式としたほうがプログラミングしやすかったので、そのような仕様になっています。


--------------------------------------------------------------------------------
## 出力

### mruby/c 提案
```
uart1.write("Output string\r\n")
```

### Ruby (gem)
IOクラスを継承しているので、IOクラスのメソッド群を使う。下記は一例。
```
uart1.write("Output.string\r\n")
```

### MicroPython
```
uart.write('abc')   # 3文字を書き込む
```

### Arduino
```
Serial.write(val)   # val: 送信する値(1バイト) 
Serial.write(str)   # str: 文字列(複数バイト) 
Serial.write(buf, len)  # buf: 配列として定義された複数のバイト, len: 配列の長さ 
```

### 所感


--------------------------------------------------------------------------------
## その他

### mruby/c 提案
```
# バッファクリア
uart1.clear_tx_buffer()
uart1.clear_rx_buffer()
```

### Ruby (gem)
```
# タイムアウト指定
read_timeout()  #  Note:Read timeouts don't mix well with multi-threading
write_timeout()
```

### MicroPython
```
any()   # ブロックなしで読み込める文字数を整数で返します。
```

### Arduino
```
Serial.flush()  # データの送信がすべて完了するまで待ちます。
```

### 所感


