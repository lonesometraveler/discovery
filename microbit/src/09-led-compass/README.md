<!-- # LED compass -->

# LEDコンパス

<!-- In this section, we'll implement a compass using the LEDs on the micro:bit. Like proper compasses, our LED
compass must point north somehow. It will do that by turning on one of its outer LEDs; the LED turned on
should point towards north. -->

この章では、micro:bitのLEDを利用してコンパスを作ります。本物のコンパスのように、私たちのコンパスも北を指し示さなくてはなりません。そのためにLEDマトリクスを利用しましょう。外縁に配置されたLEDのひとつを点灯させ、そのLEDが北を示すようにします。

<!-- Magnetic fields have both a magnitude, measured in Gauss or Teslas, and a *direction*. The
magnetometer on the micro:bit measures both the magnitude and the direction of an external magnetic field
but it reports back the *decomposition* of said field along *its axes*. -->

磁場は、ガウスやテスラで測定される大きさと、*方向*を持ちます。micro:bitの磁力計は外部磁場の大きさと方向の両方を測定しますが、その結果はmicro:bitの*軸*に沿って*分解されたもの*として提示されます。

<!-- The magnetometer has three axes associated to it. The X and Y axes basically span the plane that is the floor.
The Z axis is pointing "out" of the floor, so upwards. -->

磁力計は、みっつの軸を持っています。X軸とY軸は直角に交差して平面に伸びています。Z軸はその交点から「上に」突き出ているイメージです。

<!-- You should already be able to write a program that continuously prints the magnetometer
data on the RTT console from the [I2C chapter](../08-i2c/index.md). After you wrote that
program, locate where north is at your current location. Then line up your micro:bit with
that direction and observe how the sensor's measurements look. -->

[I2の章](../08-i2c/index.md)ですでに確認しましたから、磁力計のデータをRTTコンソールに出力し続けるプログラムは書けるはずです。そのプログラムを書き終えたら、あなたの場所での北を確認してください。それからmicro:bitをその方角にぴったりと向けて、センサがどのような値を出力するか観察してください。

<!-- Now rotate the board 90 degrees while keeping it parallel to the ground. What X, Y and Z values do
you see this time? Then rotate it 90 degrees again. What values do you see? -->

次に、ボードを地面に対して平行を保ったまま９０度回転させてください。X、Y、Z軸の値はどう変わりましたか？　もう一度９０度回転させてみましょう。値はどうなりましたか？
