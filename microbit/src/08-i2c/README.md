# I2C

<!-- We just saw the serial communication protocol. It's a widely used protocol because it's very
simple and this simplicity makes it easy to implement on top of other protocols like Bluetooth and
USB. -->

ここまでシリアル通信プロトコルを見てきました。シリアル通信はその単純さから広く使われているプロトコルです。シンプルな設計ゆえに、BluetoothやUSBのような別プロトコルの上に実装することも容易です。

<!-- However, its simplicity is also a downside. More elaborated data exchanges, like reading a digital
sensor, would require the sensor vendor to come up with another protocol on top of it. -->

ですが、単純さには欠点もあります。デジタルセンサの読み込みなど、より手の込んだデータのやり取りには別プロトコルが必要となります。

<!-- (Un)Luckily for us, there are *plenty* of other communication protocols in the embedded space. Some
of them are widely used in digital sensors. -->

幸か不幸か、組込みの世界には他にも*たくさんの*通信プロトコルが存在します。中には、デジタルセンサで広く使われているものもあります。

<!-- The micro:bit board we are using has two motion sensors in it: an accelerometer and a magnetometer.
Both of these sensors are packaged into a single component and can be accessed via an I2C bus. -->

私たちが使っているmicro:bitは、ふたつのモーションセンサを持っています。加速度計と磁力計です。その両方はひとつのコンポーネントとしてパッケージ化されており、I2Cバスでアクセスできます。

<!-- I2C stands for Inter-Integrated Circuit and is a *synchronous* *serial* communication protocol. It
uses two lines to exchange data: a data line (SDA) and a clock line (SCL). Because a clock line is
used to synchronize the communication, this is a *synchronous* protocol. -->

I2Cは、Inter-Integrated Circuitを意味し、*同期* *シリアル*通信プロトコルのひとつです。データのやり取りには、データ線（SDA）とクロック線（SCL）の二本の信号線を使います。クロック線で通信を同期させるので、*同期*プロトコルというわけです。

<p align="center">
<img class="white_bg" height=180 title="I2C bus" src="https://upload.wikimedia.org/wikipedia/commons/3/3e/I2C.svg">
</p>

<!-- This protocol uses a *master* *slave* model where the master is the device that *starts* and
drives the communication with a slave device. Several devices, both masters and slaves, can be
connected to the same bus at the same time. A master device can communicate with a specific slave
device by first broadcasting its *address* to the bus. This address can be 7 bits or 10 bits long.
Once a master has *started* a communication with a slave, no other device can make use of the bus
until the master *stops* the communication. -->

このプロトコルは*マスター・スレーブ*モデルにのっとっており、マスターが通信を*開始*し、スレーブとのやり取りをリードします。同じバスに複数のデバイス（マスターでもスレーブでも）を接続することも可能です。特定のデバイスと通信するために、マスターはまず対象となるデバイスの*アドレス*をバスにブロードキャストします。アドレスは７ビット長、あるいは１０ビット長です。一度マスターがスレーブとの通信を*開始*すると、マスターが通信を*停止*するまで他のデバイスはバスを使用できません。

<!-- The clock line determines how fast data can be exchanged and it usually operates at a frequency of
100 kHz (standard mode) or 400 kHz (fast mode). -->

クロック線が通信速度を決定します。通常は100 kHz (標準モード) か 400 kHz (ファーストモード)の周波数で動作します。
