# mruby, mruby/c 共通ペリフェラルクラス仕様提案書

## Abstract

この提案書は、mruby および mruby/c で GPIO 等のハードウェアペリフェラルを使う場合に共通で使う事ができる I/O API Class を規定する。

## License

[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.en")
<a href="https://creativecommons.org/licenses/by-sa/4.0/deed.en">
![CC BY-SA 4.0](https://komtmt.files.wordpress.com/2015/04/by-sa.png?w=150)
</a>

Copyright (C) 2022 mruby forum commitiee All rights reserved.  
Copyright (C) 2022 Kyushu Institute of Technology All rights reserved.  
Copyright (C) 2022 Shimane IT Open-Innovation Center All rights reserved.


--------------------------------------------------------------------------------
## I/O API Classes

[GPIO](mruby_io_GPIO.md): 汎用デジタル入出力  
[ADC](mruby_io_ADC.md): アナログデジタル変換  
[PWM](mruby_io_PWM.md): パルス幅変調  
[UART](mruby_io_UART.md): 非同期シリアル通信  
[I2C](mruby_io_I2C.md): I2Cシリアルバス  
[SPI](mruby_io_SPI.md): SPIシリアルバス  
