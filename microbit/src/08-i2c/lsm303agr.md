# LSM303AGR

<!-- Both of the motion sensors on the micro:bit, the magnetometer and the accelerometer, are packaged in a single
component: the LSM303AGR integrated circuit. These two sensors can be accessed via an I2C bus. Each
sensor behaves like an I2C slave and has a *different* address. -->

micro:bitが持つ磁力計と加速度計のふたつのセンサは、LSM303AGRという集積回路にひとつにまとめられており、I2Cバスでアクセス可能です。センサはそれぞれ独立したI2Cスレーブとして動作し、*別々の*アドレスを持ちます。

<!-- Each sensor has its own memory where it stores the results of sensing its environment. Our
interaction with these sensors will mainly involve reading their memory. -->

各センサは測定した環境情報をそれぞれのメモリに保存します。ですからセンサとのやりとりは、主にそのメモリの読み取りとなります。

<!-- The memory of these sensors is modeled as byte addressable registers. These sensors can be
configured too; that's done by writing to their registers. So, in a sense, these sensors are very
similar to the peripherals *inside* the microcontroller. The difference is that their registers are
not mapped into the microcontrollers' memory. Instead, their registers have to be accessed via the
I2C bus. -->

センサのメモリは、バイト単位でアドレスを指定できるレジスタの集まりとして作られています。レジスタに書き込むことで、センサの設定を変更することもできます。そういう意味では、マイクロコントローラ*内の*ペリフェラルに似ているかもしれません。異なる点は、センサのレジスタはマイクロコントローラのメモリにマッピングされておらず、I2Cバスでアクセスする必要があることです。

<!-- The main source of information about the LSM303AGR is its [Data Sheet]. Read through it to see how
one can read the sensors' registers. That part is in: -->

[Data Sheet]: https://www.st.com/resource/en/datasheet/lsm303agr.pdf

LSM303AGRについての一番の情報源は[データシート]です。どうやってセンサのレジスタを読むのかを確認してください。ここに書いてあります。

[データシート]: https://www.st.com/resource/en/datasheet/lsm303agr.pdf

> Section 6.1.1 I2C Operation - Page 38 - LSM303AGR Data Sheet

<!-- The other part of the documentation relevant to this book is the description of the registers. That
part is in: -->

他にこの本に関係のある情報といえば、レジスタについての記述でしょう。このセクションです。

> Section 8 Register description - Page 46 - LSM303AGR Data Sheet
