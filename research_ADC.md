# ADC

## コンストラクタ

### mruby/c 提案
```
ADC.new( machine dependent parameters )
adc1 = ADC.new( 1 )	    # param: grove pin number.
```

### MicroPython
```
adc = ADC(pin)        # 指定のピンで動作する ADC オブジェクトを作成
```

### Arduino  
```
なし
```

### 所感
Ruby(gem) には ADCに関わるクラスは見つけられなかった。


--------------------------------------------------------------------------------
## 入力

### mruby/c 提案
```
val = adc1.read()		# val = 0.0 - 2.048 (Volt)
```

### MicroPython
```
val = adc.read_u16()  # 生のアナログ値を 0-65535 の範囲で読込み
val = adc.read_uv()   # アナログ値をマイクロボルトで読込み
```

### Arduino
```
analogRead(pin)     # 戻り値：0から1023までの整数値
```

### 所感
MicroPython の read_v16() は、元の値をスケールして戻している。恐らく好ましくはないだろう。
