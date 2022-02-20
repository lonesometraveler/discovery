<!-- # Meet your hardware -->

# ハードウェアとの出会い

<!--
Let's get familiar with the hardware we'll be working with.
-->

これから使用するハードウェアについて知りましょう。

## micro:bit

<p align="center">
<img title="micro:bit" src="../assets/microbit-v2.jpg">
</p>

<!--
Here are some of the many components on the board:

- A [microcontroller].
- A number of LEDs, most notably the LED matrix on the back
- Two user buttons as well as a reset button (the one next to the USB port).
- One USB port.
- A sensor that is both a [magnetometer] and an [accelerometer]

[microcontroller]: https://en.wikipedia.org/wiki/Microcontroller
[accelerometer]: https://en.wikipedia.org/wiki/Accelerometer
[magnetometer]: https://en.wikipedia.org/wiki/Magnetometer
-->

ボードには多くの部品が搭載されています。

- [マイクロコントローラ]が1つ
- LEDがいくつか。特に裏面にある5x5のLED
- ユーザーボタンが2つとUSBポート横のリセットボタンが1つ
- USBポートが1つ
- [地磁気センサ]と[加速度センサ]両方を搭載しているセンサが1つ

[マイクロコントローラ]: https://en.wikipedia.org/wiki/Microcontroller
[加速度センサ]: https://en.wikipedia.org/wiki/Accelerometer
[地磁気センサ]: https://en.wikipedia.org/wiki/Magnetometer

> 訳注: 日本語Wikipediaへのリンク
> [マイクロコントローラ](https://ja.wikipedia.org/wiki/%E3%83%9E%E3%82%A4%E3%82%AF%E3%83%AD%E3%82%B3%E3%83%B3%E3%83%88%E3%83%AD%E3%83%BC%E3%83%A9)
> [加速度計](https://ja.wikipedia.org/wiki/%E5%8A%A0%E9%80%9F%E5%BA%A6%E8%A8%88)
> [磁気センサ](https://ja.wikipedia.org/wiki/%E7%A3%81%E6%B0%97%E3%82%BB%E3%83%B3%E3%82%B5)

<!--
Of these components, the most important is the microcontroller (sometimes
shortened to "MCU" for "microcontroller unit"), which is the bigger of the two
black squares sitting on the side of the board with the USB port. The MCU is
what runs your code. You might sometimes read about "programming a board", when
in reality what we are doing is programming the MCU that is installed on the board.
-->

部品の中で最も重要なものはマイクロコントローラです。マイクロコントローラは「microcontoller unit」を省略して「MCU」と呼ばれることもあります。
ボード上では、USBポートがある面に見える2つの黒い四角の大きい方です。
このMCUはあなたのコードを実行します。
「ボードにプログラムを書く」という文章を目にするかもしれませんが、実際はボードに搭載されているMCUにプログラムを書きます。

<!--
If you happen to be interested in a more in detail description of the board you
can checkout the [micro:bit website](https://tech.microbit.org/hardware/).
-->

このボードのより詳細な説明に興味がある場合、[micro:bitのウェブサイト](https://tech.microbit.org/hardware/)を訪れて見てください。

<!--
Since the MCU is so important, let's take a closer look at the one sitting on our board.
Note that only one of the following two sections applies to your board, depending on whether
you are working with a micro:bit v2 or v1.
-->

MCUは重要なので、ボードに搭載されているMCUをより詳しく見てみましょう。
次のセクションはmicro:bit v2を使うかv1を使うかで、どちらか一方を読んでください。
