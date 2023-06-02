# GPIO

## setmode

コンストラクタと同等の機能を持つクラスメソッドとしても呼べるように変更
Nilを返す事を規定。


# UART

## puts
puts メソッドの追加

## read

CRubyとの互換性を重視し、ブロックするように変更。

## メソッド削除

read_nonblock

## メソッド追加

bytes_avallable
can_read_line
bytes_to_write

## メソッド名前変更

break -> send_break


# I2C

## read, write
stop: Boolean パラメータの廃止

## メソッド追加

以下のローレベルメソッドの追加
 * send_start
 * send_restart
 * send_stop
 * raw_read
 * raw_write


# SPI

## メソッド追加
setmode
transfer

## write
戻り値の Integer を取りやめ、Nilにする。
実装に結構なコストがかかるのにもかかわらず、SPI バスの性格上、指定したデータが必ず送信されるため、戻り値にはほとんど意味がない。
