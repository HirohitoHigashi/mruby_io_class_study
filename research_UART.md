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
uart.init(9600, bits=8, parity=None, stop=1) # 与えたパラメータで初期化
```

### Arduino
```
なし
```

### 所感
mruby/c 提案は、Rubygemからもらってきたもの。  
GPIOと同じように、コンストラクタで全て指定してしまえば良いが、UARTは通信途中でパラメータ（ボーレート）を変更するケースがあるので、設定用メソッドがあった方が良い。
