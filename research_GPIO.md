# GPIO

## コンストラクタ

### mruby/c 提案
```
obj = GPIO.new( machine dependent parameters )
```

### Ruby (gem)
ruby-gpio 0.0.2 https://rubygems.org/gems/ruby-gpio
```
GPIO.new( name, number )
```

### MicroPython
```
# ピン #0 を出力ピンとして作成
p0 = Pin(0, Pin.OUT)
```

### Arduino  
```
なし
```

### 所感
クラス名は、GPIOの方が良い。  
MicroPythonのように、「どのピンか？」及び「その属性」を、コンストラクタで指定できた方が良さそうだ。  
公開中のmruby/c I/O API仕様では、「（参考）機器によってはコンストラクタの引数で入出力の設定も併せて行い、実行途中で
は変更できないという方法が良いでしょう。」 として、同様の事を示唆しているが、実装はしていない。


--------------------------------------------------------------------------------
## 設定

### mruby/c 提案
```
obj.setmode( GPIO::IN )
obj.setmode( GPIO::OUT )
```

### Ruby (gem)
```
obj.as( :in )
obj.as( :out )
```

### MicroPython
```
p0.init(p0.IN, p0.PULL_DOWN)
```

### Arduino  
```
pinMode( pin, mode )    // pin=0...  mode = INPUT,OUTPUT,INPUT_PULLUP
```

### 所感
setmodeの名前は、RPi.GPIO (Python) から取ったかもしれないが、今調べると役割が違っていた。


--------------------------------------------------------------------------------
## 入力

### mruby/c 提案
```
val = obj.read()            # val = 0 or 1
```

### Ruby (gem)
```
val = obj.read()            # val = 0 or 1
```

### MicroPython
```
val = p2.value()
```

### Arduino  
```
digitalRead(pin) 
```

### 所感
MicroPython は、後述する出力にも同名のメソッド value を使っており、引数の有無で動作を変える仕様は、あまり好ましくないと考える。
一般的に、読み込みは気軽に可能で、書き込みは注意が必要と思わせるような設計にすべき。



--------------------------------------------------------------------------------
## 出力

### mruby/c 提案
```
obj.write( 1 )
```

### Ruby (gem)
```
obj.off()   # output Low
obj.on()    # output High
```

### MicroPython
```
p0.value(0)
p0.on()
p0.off()
p0.low()
p0.high()
```

### Arduino  
```
digitalWrite(pin, HIGH)
digitalWrite(pin, LOW)
```

### 所感
Ruby(gem)のようにメソッド名で値を区別する方法は、使いにくい場合が多い。やはり値は引数で指定すべき。  
ハードウェアは負論理で設計されることも多いので、単純なON/OFFが逆の意味を持つことがある。そのため、例えば Ruby(gem)の仕様では 「 `motor_switch.off()` をコールしたらモーターが回り出す」等の状況がありうる。  
値は、1 == High == 3.3V(例) との暗黙の了解がある。正論理負論理の話も、さすがにこのレイヤーで 1 がハイレベルを表すことを疑問に思う人はいないだろう。  
