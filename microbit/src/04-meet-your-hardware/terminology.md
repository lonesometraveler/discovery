<!--
# Rust Embedded terminology
Before we dive into programming the micro:bit let's have a quick look
at the libraries and terminology that will be important for all the
future chapters.
-->

# 組込みRustの専門用語
micro:bitでのプログラミングを始める前に、今後の章で重要となるライブラリと専門用語とを一通り確認しておきましょう。

<!--
## Abstraction layers
For any fully supported microcontroller/board with a microcontroller
you will usually hear the following terms being used for their levels
of abstraction:
-->

## 抽象化レイヤー
フルサポートされているマイクロコントローラーあるいはボードに対して、その抽象化レベルに応じて、次の用語を目にするでしょう。

<!--
### Peripheral Access Crate (PAC)
The job of the PAC is to provide a safe (ish) direct interface to the
peripherals of the chip, allowing you to configure
every last bit however you want (of course also in wrong ways). Usually
you only ever have to deal with the PAC if either the layers that are
higher up don't fulfill your needs or when you are developing them.
The PAC we are (implicitly) going to use is either the one for the [nRF52]
or for the [nRF51].
-->

### ペリフェラルアクセスクレート (PAC; Peripheral Access Crate)
PACはチップ上のペリフェラルへの安全（と思われる）なインタフェースを提供し、あらゆるビットを好きなように設定できます（もちろん間違った使い方もできます）。
通常、より上位レイヤーでやりたいことができない場合か上位レイヤー自体を開発する時のみ、PACを扱います。
この本で（ひそかに）使うPACは[nRF52]向けのものか[nRF51]向けのものです。

<!--
### The Hardware Abstraction Layer (HAL)
The job of the HAL is to build up on top of
the chip's PAC and provide an abstraction that is actually usable for
someone who does not know about all the special behaviour of this chip.
Usually they abstract whole peripherals away into single structs that can
for example be used to send data around via the peripheral. We are
going to use the [nRF52-hal] or the [nRF51-hal] respectively.
-->

### ハードウェア抽象化レイヤー (HAL; The Hardware Abstraction Layer)
HALはチップのPACの上位レイヤーを作り上げ、このチップ特有の振る舞いをあまり知らない人にも便利な抽象を提供します。
通常、HALは全てのペリフェラルをそれぞれ1つの構造体で抽象化します。
例えば、ペリフェラル経由でデータを送信するために使える構造体があります。
この本では[nRF52-hal]もしくは[nRF51-hal]をそれぞれ使います。

<!--
### The Board Support Crate (historically called Board Support Package, or BSP)
The job of the BSP is to abstract a whole board
(such as the micro:bit) away at once. That means it has to provide
abstractions to use both the microcontroller as well as the sensors,
LEDs etc. that might be present on the board. Quite often (especially
with custom-made boards) you will just be working with a HAL for the
chip and build the drivers for the sensors either yourself or
search for them on crates.io. Luckily for us though, the micro:bit
does actually have a [BSP] so we are going to use that on top of our
HAL as well.
-->

### ボードサポートクレート (ボードサポートパッケージもしくはBSPと呼ばれます)
BSPは（micro:bitのような）ボード全体を抽象化します。
BSPはマイクロコントローラだけでなくセンサーやLEDなど、ボード上に存在するものを使えるように抽象を提供します。
頻繁に（特にカスタムされたボードでは）そのチップのHALを使ってセンサーのドライバを自分で作成するか、crates.ioを探すことになります。
幸い、micro:bitには[BSP]があるため、HALの上に作られたBSPを使います。

[nrF52]: https://crates.io/crates/nrf52833-pac
[nrF51]: https://crates.io/crates/nrf51
[nrF52-hal]: https://crates.io/crates/nrf52833-hal
[nrF51-hal]: https://crates.io/crates/nrf51-hal
[BSP]: https://crates.io/crates/microbit

<!--
## Unifying the layers

Next we are going to have a look at a very central piece of software
in the Rust Embedded world: [`embedded-hal`]. As its name suggests it
relates to the 2nd level of abstraction we got to know: the HALs.
The idea behind [`embedded-hal`] is to provide a set of traits that
describe behaviour which is usually shared across all implementations
of a specific peripheral in all the HALs. For example one would always
expect to have functions that are capable of turning the power on a pin
either on or off. For example to switch an LED on and off on the board.
This allows us to write a driver for, say a temperature sensor, that
can be used on any chip for which an implementation of the [`embedded-hal`] traits exists,
simply by writing the driver in such a way that it only relies on the
[`embedded-hal`] traits. Drivers that are written in such a way are called
platform agnostic and luckily for us most of the drivers on crates.io
are actually platform agnostic.
-->

## レイヤーを統合する

次に、組込みRustの世界でまさに中心の役割を担う[`embedded-hal`]について見ていきます。
名前の通り、[`embedded-hal`]は2レベル目の抽象化であるHALに関係しています。
[`embedded-hal`]の根底にあるアイデアは、HALに含まれる特定ペリフェラル実装で共通となる振る舞いを定義したトレイト一式を提供することです。
例えば、ボード上のLED追加をオン、オフするために、あるピンの電源をオン、オフできる機能は必ずあるはずです。
共通となる振る舞いをトレイトとして定義することで、[`embedded-hal`]トレイトの実装が存在するチップであれば、[`embedded-hal`]のトレイトにのみ依存するようにドライバを書くだけで、どのチップでも使えるドライバ（例えば温度センサのドライバ）を書くことができます。
このように書かれたドライバをプラットフォーム非依存 (platform agnostic) と呼びます。
嬉しいことに、crates.ioにある多くのドライバが実際にプラットフォーム非依存です。

[`embedded-hal`]: https://crates.io/crates/embedded-hal

<!--
## Further reading

If you want to learn more about these levels of abstraction, Franz Skarman,
a.k.a. [TheZoq2], held a talk about this topic during Oxidize 2020, called
[An Overview of the Embedded Rust Ecosystem].
-->

## さらに詳しく

もし紹介した抽象レベルについてより詳しく知りたい場合、Franz Skarman ([TheZoq2]) がOxidize 2020の[An Overview of the Embedded Rust Ecosystem]でこのトピックについて話しています。

[TheZoq2]: https://github.com/TheZoq2/
[An Overview of the Embedded Rust Ecosystem]: https://www.youtube.com/watch?v=vLYit_HHPaY
