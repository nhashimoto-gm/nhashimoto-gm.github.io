---
layout: default
---

# BNO055_i2c_RPi

[https://github.com/nhashimoto-gm/BNO055_i2c_RPi](https://github.com/nhashimoto-gm/BNO055_i2c_RPi)

## 使用センサー
Bosch Sensortec GmbH Smart sensor: BNO055

[https://www.bosch-sensortec.com/products/smart-sensors/bno055/](https://www.bosch-sensortec.com/products/smart-sensors/bno055/)

Adafruit BNO055 Absolute Orientation Sensor

[https://learn.adafruit.com/adafruit-bno055-absolute-orientation-sensor](https://learn.adafruit.com/adafruit-bno055-absolute-orientation-sensor)

## 接続機器と接続方法
RaspberryPIにi2c接続で通信。

以下の通信速度で使用したほうがデータ取得安定します。

標準のbaudrate=100000では、" OSError: [Errno 121] Remote I/O error " が頻発しました。

- dtparam=i2c_arm_baudrate=50000

(留意点) プルアップ抵抗は不要です。

## 注意
LOCAL NETWORK上のInfluxdb v1.8サーバーへデータを送信。

InfluxQLは以下のような形で情報取得。(Grafana等利用)
```
SELECT mean("eu_x-axis") FROM "bno055_measure_euler" WHERE $timeFilter GROUP BY time($__interval) fill(none)
```

## Acknowledgments ( 謝辞 )

ghirlekar, you have been very helpful.

[https://github.com/ghirlekar/bno055-python-i2c](https://github.com/ghirlekar/bno055-python-i2c)

## --------------------------------------------------------------------------

# ADXL355_i2c_RPi

[https://github.com/nhashimoto-gm/ADXL355_i2c_RPi](https://github.com/nhashimoto-gm/ADXL355_i2c_RPi)

## 使用センサー
ANALOG DEVICES ADXL355

[https://www.analog.com/jp/products/adxl355.html](https://www.analog.com/jp/products/adxl355.html)

Strawberry Linux Co.,Ltd. ADXL355 超低ノイズ３軸加速度センサモジュール（ディジタル出力）2g/4g/8g

[http://strawberry-linux.com/catalog/items?code=12355](http://strawberry-linux.com/catalog/items?code=12355)

## 接続機器と接続方法
RaspberryPIにi2c接続で通信。

(留意点１) 3.3VをVSUPPLYとVDDIOに供給。SCLK/VSSIOとMISO/ASELはGNDに接続。

i2cアドレスを0x1Dとしています。( MISO/ASELがLow ) ※Highで0x53となる。

(留意点２) プルアップ抵抗は3k-5kΩ。( 3.7kΩだったかな )

## 測定レンジについて
range2G設定にしてあります。( 256,000LSB/g ±4.096g-range )

但し、算出数値の単位は加速度:m/s<sup>2</sup>としている。

この場合の計算式は、以下のとおり。( 前提として、重力加速度 g = 9.80665 m/s<sup>2</sup> )

```
"x-axis":allAxes['x']*g/256000.0,"y-axis":allAxes['y']*g/256000.0,"z-axis":allAxes['z']*g/256000.0
```
>range2G設定 -> 256,000LSB/g ±2.048g-range
>
>range4G設定 -> 128,000LSB/g ±4.096g-range
>
>range8G設定 ->  64,000LSB/g ±8.192g-range

## 注意
最初はprint文をアンコメントし、Influxdb書き込み部分をコメントアウトして要確認。

LOCAL NETWORK上のInfluxdb v1.8サーバーへデータを送信。

InfluxQLは以下のような形で情報取得。(Grafana等利用)
```
SELECT mean("x-axis") FROM "autogen"."adxl355_measure" WHERE $timeFilter GROUP BY time($__interval) fill(none)
```

## Acknowledgments ( 謝辞 )

Markrad, you have been very helpful.

[https://github.com/markrad/ADXL355](https://github.com/markrad/ADXL355)




```
Text can be **bold**, _italic_, or ~~strikethrough~~.

# Header 1

This is a normal paragraph following a header. 

GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}


ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.githubassets.com/images/icons/emoji/octocat.png)

### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.

```
