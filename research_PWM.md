# PWM

## コンストラクタ

### mruby/c 提案
```
pwm = PWM.new( machine dependent parameters )
```

### Ruby (gem)
なし？

### MicroPython
```
# PWM(dest, *, freq, duty_u16, duty_ns)
pwm = PWM(pin)          # 指定のピンの PWM オブジェクトを作成
```

### Arduino  
なし。

### 所感
他のクラスと同じように、コンストラクタで初期のパラメータを指定できた方が良いかもしれない。


--------------------------------------------------------------------------------
## 周波数設定

### mruby/c 提案
```
pwm.frequency( 440 )    # 440Hz
pwm.period_us( 2273 )   # 周期を指定する方法　1/2273us = 440Hz
```

### MicroPython
```
pwm.freq(5000)

# 200us の周期、デューティ比を 5us で再初期化
pwm.init(freq=5000, duty_ns=5000)
```

### Arduino
なし？

### 所感
周波数とデューティーを両方同時に変更できるメソッドもあった方が良いとは思うが、両方同時に変更が必要なアプリケーションには何があるだろうか？  


--------------------------------------------------------------------------------
## デューティー比設定

### mruby/c 提案
```
pwm.duty( 512 )        # 50%
```

### MicroPython
```
pwm.duty_u16([value])   # PWM 出力の現在のパルス幅を、0〜65535 の範囲の符号なし16ビット値として取得または設定します。
pwm.duty_ns([value])    # PWM 出力の現在のパルス幅をナノ秒単位の値として取得または設定します。
```

### Arduino
```
# Writes an analog value (PWM wave) to a pin.
analogWrite(pin, value)  # value 0 to 255
```

### 所感
Arduinoの、「PWMはアナログ値」の割り切りがすごい。  
mruby/c の、1024を最大値（100%)にするルールは、何かを真似たものだったはずだけれど、何だったっか忘れた。  
MicroPythonの真似がよさそうに思う。  

