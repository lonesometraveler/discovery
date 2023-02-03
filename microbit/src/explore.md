<!-- # What's left for you to explore -->

# もっと楽しむために

<!-- We have barely scratched the surface! There's lots of stuff left for you to
explore. -->

組込みRustの入門を終えましたが、旅はまだ始まったばかりです。この先にもっと楽しい世界が広がっています！

<!-- 
> **NOTE:** If you're reading this, and you'd like to help add examples or
> exercises to the Discovery book for any of the items below, or any other
> relevant embedded topics, we'd love to have your help!
>
> Please [open an issue] if you would like to help, but need assistance or
> mentoring for how to contribute this to the book, or open a Pull Request
> adding the information!

[open an issue]: https://github.com/rust-embedded/discovery/issues/new
-->

> **注:** この本に貢献していただけるならうれしいです！　以下の項目や、ほかにも組込みに関するトピックについて、サンプルコードや課題を追加する作業をお手伝い頂ける方はぜひ手を貸してください。
>
> 貢献するにあたってガイドが必要な場合は、[イシューを立ててください]。ご自分で情報を追加できるようなら、プルリクエストを送ってくださっても結構です。

[イシューを立ててください]: https://github.com/rust-embedded/discovery/issues/new

<!-- ## Topics about embedded software -->

## 組込みソフトウェアができること

<!-- These topics discuss strategies for writing embedded software. Although many
problems can be solved in different ways, these sections talk about some
strategies, and when they make sense (or don't make sense) to use. -->

以下は、組込みソフトウェアを書くにあたっての設計にかかわるトピックです。多くの問題は、いろいろなやり方で解決できます。ここではいくつかのやり方を紹介し、それぞれの向き不向きについて解説します。

<!-- ### Multitasking -->

### マルチタスク

<!-- All our programs executed a single task. How could we achieve multitasking in a
system with no OS, and thus no threads. There are two main approaches to
multitasking: preemptive multitasking and cooperative multitasking. -->

私たちが作ったプログラムはすべてシングルタスクでした。もしもOSもスレッドもない環境でマルチタスクを実現するとしたら、どうするでしょうか。よく使われる方法がふたつあります。プリエンプティブ・マルチタスクと協調的マルチタスクです。

<!-- In preemptive multitasking a task that's currently being executed can, at any point in time, be
*preempted* (interrupted) by another task. On preemption, the first task will be suspended and the
processor will instead execute the second task. At some point the first task will be resumed.
Microcontrollers provide hardware support for preemption in the form of *interrupts*. -->

プリエンプティブ・マルチタスクでは、現在実行中のタスクは他のタスクにいつでも*プリエンプト*される（強制的に差し替えられる）可能性があります。もともとのタスクは一時停止され、プロセッサは別のタスクを実行します。そしてあとになってから最初のタスクを再開します。マイクロコントローラは*割り込み*のかたちでプリエンプションをハードウェアサポートしています。

<!-- In cooperative multitasking a task that's being executed will run until it reaches a *suspension
point*. When the processor reaches that suspension point it will stop executing the current task and
instead go and execute a different task. At some point the first task will be resumed. The main
difference between these two approaches to multitasking is that in cooperative multitasking *yields*
execution control at *known* suspension points instead of being forcefully preempted at any point of
its execution. -->

協調的マルチタスクでは、実行中のタスクは*中断点*に到達するまで実行を続けます。そしてその中断点に到達すると、タスクを一時停止し、別のタスクを実行します。そしてあとになって最初のタスクを再開します。ふたつのマルチタスクの大きな違いは、協調的マルチタスクは実行をいつでも強制的に差し替えるのではなく、*あらかじめ知られた*中断点において実行制御を*譲る*ということです。

<!-- ### Sleeping -->

### スリープ

<!-- All our programs have been continuously polling peripherals to see if there's
anything that needs to be done. However, sometimes there's nothing to be done!
At those times, the microcontroller should "sleep". -->

この本のすべてのプログラムは、新しい処理が必要かどうかペリフェラルをポーリングしていました。ですが、なにもする必要がないときだってあります。そんなときにはマイクロコントローラは「スリープ」すべきです。

<!-- When the processor sleeps, it stops executing instructions and this saves power.
It's almost always a good idea to save power so your microcontroller should be
sleeping as much as possible. But, how does it know when it has to wake up to
perform some action? "Interrupts" (see below for what exactly those are)
are one of the events that wake up the microcontroller but there are others
and the `wfi` and `wfe` are the instructions that make the processor "sleep". -->

プロセッサがスリープすると、命令の実行が止まり、電力を節約できます。マイクロコントローラはできるかぎりスリープさせるべきです。ですが、どうやっていつ起きればよいと判断するのでしょうか？　「割り込み」（詳細は下をご覧ください）でマイクロコントローラを起こすことができますが、他にも方法があります。また、プロセッサを「スリープ」させる命令は`wfi`と`wfe`となります。

<!-- ## Topics related to microcontroller capabilities -->

## マイクロコントローラができること

<!-- Microcontrollers (like our nRF52/nRF51) have many capabilities. However, many share similar
capabilities that can be used to solve all sorts of different problems. -->

nRF52やnRF51などのマイクロコントローラにはできることがたくさんあります。そのなかでも、さまざまな問題を解決するためによく使われる機能があります。

<!-- These topics discuss some of those capabilities, and how they can be used effectively
in embedded development. -->

以下では、そういった機能を組込み開発でいかに効果的に使うかを解説します。

<!-- ### Direct Memory Access (DMA). -->

### ダイレクトメモリアクセス (DMA)

<!-- This peripheral is a kind of *asynchronous* `memcpy`. If you are working with
a micro:bit v2 you have actually already used this, the HAL does this for you
with the UARTE and TWIM peripherals. A DMA peripheral can be used to perform bulk
transfers of data. Either from RAM to RAM, from a peripheral, like a UARTE, to RAM
or from RAM to a peripheral. You can schedule a DMA transfer, like read 256 bytes
from UARTE into this buffer, leave it running in the background and then poll some
register to see if it has completed so you can do other stuff while the transfer
is ongoing. For more information as to how this is implemented you can checkout the
`serial_setup` module from the UART chapter. If that isn't enough yet you could even
try and dive into the code of the [`nrf52-hal`]. -->

このペリフェラルは*非同期の* `memcpy`と言っていいでしょう。もしもmicro:bit v2をお使いでしたら、すでにDMAを使ったことになります。なぜならHALがUARTEとTWIMペリフェラルにこの機能を利用しているからです。DMAペリフェラルは、まとまったデータの転送に使われます。RAMからRAMへも、UARTEのようなペリフェラルからRAMへも、RAMからペリフェラルへも使えます。たとえば256バイトをUARTEからバッファに読み込むとしましょう。DMA転送をスケジュールすれば、バックグラウンドで処理を行うことができます。処理の終了はレジスタをポーリングすることで確認し、転送作業中はその終了を待たずに他のタスクを実行できるのです。これがどうやって実装されているのか興味があれば、UART の章で見た`serial_setup`モジュールをのぞいてみてください。それでも満足できなければ、[`nrf52-hal`]のコードをどうぞ。

[`nrf52-hal`]: https://github.com/nrf-rs/nrf-hal

<!-- ### Interrupts -->

### 割り込み

<!-- In order to interact with the real world, it is often necessary for the
microcontroller to respond *immediately* when some kind of event occurs. -->

マイクロコントローラが実世界とやり取りするとき、なにかイベントが起きたときに*即座に*反応しなくてはならないことがよくあります。

<!-- Microcontrollers have the ability to be interrupted, meaning when a certain event
occurs, it will stop whatever it is doing at the moment, to instead respond to that
event. This can be very useful when we want to stop a motor when a button is pressed,
or measure a sensor when a timer finishes counting down. -->

マイクロコントローラには割り込み機能があります。つまりなにかイベントが起きたときに、実行中の処理を中断し、そのイベントに反応するのです。たとえば、ボタンが押されると即座にモーターを止めたいとか、タイマのカウントダウンが終わったらセンサに測定させたいとかいったときに便利です。

<!-- Although these interrupts can be very useful, they can also be a bit difficult
to work with properly. We want to make sure that we respond to events quickly,
but also allow other work to continue as well. -->

割り込みはとても便利なものですが、適切に扱うのは簡単ではありません。イベントにすばやく反応はしたいのですが、他の処理も続けたいところです。

<!-- In Rust, we model interrupts similar to the concept of threading on desktop Rust
programs. This means we also must think about the Rust concepts of `Send` and `Sync`
when sharing data between our main application, and code that executes as part of
handling an interrupt event. -->

Rustでは、デスクトップアプリケーションにおけるスレッドの概念に似せて割り込みを設計しています。それはつまり、割り込み処理を実行するコードとメインアプリケーションとでデータを共有するとき、`Send`と`Sync`についてよく考えなくてはならないということも意味します。


<!--### Pulse Width Modulation (PWM)-->

### パルス幅変調 (PWM)

<!--In a nutshell, PWM is turning on something and then turning it off periodically
while keeping some proportion ("duty cycle") between the "on time" and the "off
time". When used on a LED with a sufficiently high frequency, this can be used
to dim the LED. A low duty cycle, say 10% on time and 90% off time, will make
the LED very dim wheres a high duty cycle, say 90% on time and 10% off time,
will make the LED much brighter (almost as if it were fully powered).-->

簡単に言うと、PWMとは「オンの時間」と「オフの時間」を一定の割合（デューティー比）で保ちながら、周期的にオン、オフを繰り返すことです。十分高い周波数でこれをLEDに使うと、調光することもできます。低いデューティー比、たとえばオンの時間10％、オフの時間90％ではLEDはとても暗く光ります。それに対し、オンの時間90％、オフの時間10％といった高いデューティー比ではLEDはずっと明るく（ほとんど全灯に思えるほどに）なります。

<!--In general, PWM can be used to control how much *power* is given to some
electric device. With proper (power) electronics between a microcontroller and
an electrical motor, PWM can be used to control how much power is given to the
motor thus it can be used to control its torque and speed. Then you can add an
angular position sensor and you got yourself a closed loop controller that can
control the position of the motor at different loads.-->

一般的に、PWMはどれだけの*電力*を電子機器に与えるかを制御するのに使われます。適切な駆動回路を挟めば、PWMを利用してマイクロコントローラでモーターの操作もできます。どれだけの電力を供給するかを調整することで、モーターのトルクとスピードを制御できるのです。さらに角度センサを加えれば、モーターを自動制御するシステムを作ることもできます。

<!--PWM is already abstracted within the [`embedded-hal` `Pwm` trait] and you will
again find implementations of this in the [`nrf52-hal`].-->

PWMはすでに[`embedded-hal` `Pwm` トレイト]によって抽象化されています。その実装は[`nrf52-hal`]をご覧ください。

<!--[`embedded-hal` `Pwm` trait]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/trait.Pwm.html-->

[`embedded-hal` `Pwm` トレイト]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/trait.Pwm.html


<!--### Digital inputs-->

### デジタル入力

<!--We have used the microcontroller pins as digital outputs, to drive LEDs. But
these pins can also be configured as digital inputs. As digital inputs, these
pins can read the binary state of switches (on/off) or buttons (pressed/not
pressed).
-->

この本では、LEDを制御するためにマイクロコントローラのピンをデジタル出力として扱ってきました。ですが、ピンはデジタル入力として設定することもできます。デジタル入力ピンは、スイッチ（オン/オフ）やボタン（押された/押されていない）の二値状態を読むことができます。

<!--Again digital inputs are abstracted within the [`embedded-hal` `InputPin` trait]
and of course the [`nrf52-hal`] does have an implementation for them.-->

デジタル入力もまた[`embedded-hal` `InputPin` トレイト]で抽象化されています。もちろん[`nrf52-hal`]がそれを実装しています。

<!--(*spoilers* reading the binary state of switches / buttons is not as
straightforward as it sounds ;-) )-->

（*ネタバレ*　スイッチやボタンの二値状態を読むのは案外簡単ではありません。;-) )

<!--[`embedded-hal` `InputPin` trait]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/digital/v2/trait.InputPin.html
-->

[`embedded-hal` `InputPin` トレイト]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/digital/v2/trait.InputPin.html

<!--### Analog-to-Digital Converters (ADC)-->

### アナログデジタルコンバータ (ADC)

<!--There are a lot of digital sensors out there. You can use a protocol like I2C
and SPI to read them. But analog sensors also exist! These sensors just output a
voltage level that's proportional to the magnitude they are sensing.-->

世の中にはたくさんのデジタルセンサがあり、I2CやSPIプロトコルを使ってセンサからデータを読み取ります。ですが、アナログセンサもあります！　これらのセンサは検知するものの大きさに比例した電圧を出力します。

<!--The ADC peripheral can be used to convert that "analog" voltage level, say `1.25`
Volts, into a "digital" number, say in the `[0, 65535]` range, that the processor
can use in its calculations.
-->
ADCペリフェラルは、たとえば`1.25`ボルトといった「アナログな」電圧レベルを、プロセッサが計算に使える`[0, 65535]`に収まる「デジタルな」数値に置き換えます。

<!--Again the [`embedded-hal` `adc` module] as well as the [`nrf52-hal`] got you covered.-->

ここでもまた、[`embedded-hal` `adc` モジュール]と[`nrf52-hal`]が抽象化と実装をしてくれています。

<!--[`embedded-hal` `adc` module]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/adc/index.html-->

[`embedded-hal` `adc` モジュール]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/adc/index.html

<!--### Digital-to-Analog Converters (DAC)-->

### デジタルアナログコンバータ (DAC)

<!--As you might expect a DAC is exactly the opposite of ADC. You can write some
digital value into a register to produce a voltage in the `[0, 3.3V]` range
(assuming a `3.3V` power supply) on some "analog" pin. When this analog pin is
connected to some appropriate electronics and the register is written to at some
constant, fast rate (frequency) with the right values you can produce sounds or
even music!-->

お気づきかもしれませんが、DACはADCの反対です。デジタルな値をレジスタに書き込むことで、`[0, 3.3V]`（`3.3V`電源と想定）の範囲に収まる電圧を「アナログ」ピンから出力することができます。このアナログピンを適切な回路に接続し、レジスタに一定の高い周波数で適当な値を書き込めば、音を発生させることも音楽を奏でることだってできます！

<!--### Real Time Clock (RTC)-->

### リアルタイムクロック (RTC)

<!--This peripheral can be used to track time in "human format". Seconds, minutes,
hours, days, months and years. This peripheral handles the translation from
"ticks" to these human friendly units of time. It even handles leap years and
Daylight Save Time for you!-->

このペリフェラルは、「人にわかりやすいかたちで」時間を追ってくれます。「ティック」を秒、分、時、日、月、年といった単位に変換します。うるう年や夏時間にも対応できます！


<!--### Other communication protocols-->

### その他の通信プロトコル

<!--- SPI, abstracted within the [`embedded-hal` `spi` module] and implemented by the [`nrf52-hal`]
- I2S, currently not abstracted within the `embedded-hal` but implemented by the [`nrf52-hal`]
- Ethernet, there does exist a small TCP/IP stack named [`smoltcp`] which is implemented for some
  chips but the ones on the micro:bit don't feature an Ethernet peripheral
- USB, there is some experimental work on this, for example with the [`usb-device`] crate
- Bluetooth, there does exist an incomplete BLE stack named [`rubble`] which does support nrf chips.
- SMBUS, neither abstracted in `embedded-hal` nor implemented by the [`nrf52-hal`] at the moment.
- CAN, neither abstracted in `embedded-hal` nor implemented by the [`nrf52-hal`] at the moment
- IrDA, neither abstracted in `embedded-hal` nor implemented by the [`nrf52-hal`] at the moment-->

- SPI：[`embedded-hal` `spi` モジュール]によって抽象化、[`nrf52-hal`]にて実装されてます。
- I2S：現時点では `embedded-hal`による抽象化はされていませんが、[`nrf52-hal`]が実装しています。
- Ethernet：[`smoltcp`]という軽量のTCP/IPスタックがあり、いくつかのチップでは実装されています。ですが、micro:bitのチップにはEthernetペリフェラルがありません。
- USB：[`usb-device`]クレートなど、実験的な試みはあります。
- Bluetooth：開発途中のものですが、[`rubble`]というBLEスタックがあり、nrfチップもサポートしています。
- SMBUS：現時点では、`embedded-hal`による抽象化も[`nrf52-hal`]による実装もされていません。
- CAN：現時点では、`embedded-hal`による抽象化も[`nrf52-hal`]による実装もされていません。
- IrDA：現時点では、`embedded-hal`による抽象化も[`nrf52-hal`]による実装もされていません。

<!--[`embedded-hal` `spi` module]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/spi/index.html-->
[`embedded-hal` `spi` モジュール]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/spi/index.html
[`smoltcp`]: https://github.com/smoltcp-rs/smoltcp
[`usb-device`]: https://github.com/mvirkkunen/usb-device
[`rubble`]: https://github.com/jonas-schievink/rubble

<!--Different applications use different communication protocols. User facing
applications usually have a USB connector because USB is a ubiquitous
protocol in PCs and smartphones. Whereas inside cars you'll find plenty of CAN
"buses". Some digital sensors use SPI, others use I2C and others, SMBUS.-->

アプリケーションによって使用する通信プロトコルは変わってきます。ユーザーとのインターフェースになるアプリケーションは通常USBコネクターを持っています。USBがPCやスマートフォンのありとあらゆるところで使われているプロトコルだからです。それに対し、車のなかはCAN「バス」であふれていることでしょう。デジタルセンサによってSPIを使ったり、I2Cを使ったり、SMBUSを使うものもあります。

<!--If you happen to be interested in developing abstractions in the `embedded-hal` or
implementations of peripherals in general, don't be shy to open an issue in the HAL
repositories. Alternatively you could also join the [Rust Embedded matrix channel]
and get into contact with most of the people who built the stuff from above.-->

もしも`embedded-hal`での抽象化作業や、ペリフェラルの実装に興味があれば、遠慮せずにHALのリポジトリでイシューを立ててください。また、[Rust Embedded matrix channel]に参加してもらってもいいです。上記のモジュールの開発者のほとんどとやり取りできます。


<!--## General Embedded-Relevant Topics-->

## 組込みシステム一般についてのトピック

<!--These topics cover items that are not specific to our device, or the hardware on
it. Instead, they discuss useful techniques that could be used on embedded
systems.-->

以下は、micro:bitやその上に載っているハードウェアについてではなく、組込みシステム開発における便利なテクニックを紹介します。

<!--### Gyroscopes-->

### ジャイロスコープ

<!--As part of our Punch-o-meter exercise, we used the Accelerometer to measure
changes in acceleration in three dimensions. But there are other motion
sensors such as gyroscopes, which allows us to measure changes in "spin" in three
dimensions.-->

パンチングマシンの課題では、加速度計を使って三軸における加速度の変化を計測しました。しかし、モーションセンサは他にもあります。ジャイロスコープはそのひとつで、「回転」を三次元で計測することができます。

<!--This can be very useful when trying to build certain systems, such as a robot
that wants to avoid tipping over. Additionally, the data from a sensor like a
gyroscope can also be combined with data from accelerometer using a technique
called Sensor Fusion (see below for more information).-->

この機能は、たとえばロボットの転倒を防ぎたいときなどにはとても便利です。さらに、ジャイロスコープからのデータと加速度計からのデータとを合わせて、センサフュージョンと呼ばれるテクニックを使うこともできます。（詳細は下を参照してください）

<!--### Servo and Stepper Motors-->

### サーボモーターとステッピングモーター

<!--While some motors are used primarily just to spin in one direction or the other,
for example driving a remote control car forwards or backwards, it is sometimes
useful to measure more precisely how a motor rotates.-->

たとえばリモコンカーを進めたりバックさせたりと、モーターはただ一方向に回ればいいというときもありますが、ときにはモーターを決まった角度だけ正確に動かしたいこともあります。

<!--Our microcontroller can be used to drive Servo or Stepper motors, which allow
for more precise control of how many turns are being made by the motor, or
can even position the motor in one specific place, for example if we wanted to
move the arms of a clock to a particular direction.-->

より正確な操作が可能なサーボモーターとステッピングモーターというものがあります。マイクロコントローラを使い、決まった方向に決まった角度だけ動かしたり、あるポジションで止めることができます。これを利用して、たとえば時計の針を動かすとかいったことができます。

<!--### Sensor fusion-->

### センサフュージョン

<!--The micro:bit contains two motion sensors: an accelerometer and a magnetometer.
On their own these measure: (proper) acceleration and (the Earth's) magnetic field.
But these magnitudes can be "fused" into something more useful: a "robust" measurement
of the orientation of the board. Where robust means with less measurement error than
a single sensor would be capable of.-->

micro:bitはふたつのモーションセンサを搭載しています。加速度計と磁力計です。それぞれは（固有）加速度と、（地球の）磁場を計測しています。ですが、これらの大きさを「まとめて」もっと有用なデータとすることができます。つまり、ボードの向きについて「信頼性の高い」計測をすることができます。ここでの「信頼性を高める」とは、ひとつのセンサでは防ぐことのできないエラーを減らすという意味です。

<!--This idea of deriving more reliable data from different sources is known as
sensor fusion.-->

このように異なるセンサからのデータを集めてより信頼性の高いデータを取得することを、センサフュージョンと呼びます。

---

<!--So where to next? There are several options:-->

さて、次はなにに取り組みましょうか？　いくつか提案です。

<!--- You could check out the examples in the [`microbit`] board support crate. All those examples work for
  the micro:bit board you have.-->
  
- [`microbit`]ボードサポートクレートについてくるサンプルコードを試してみるといいかもしれません。コードはすべてお持ちのmicro:bitで動作します。

[`microbit`]: https://github.com/nrf-rs/microbit/

<!--- You could join the [Rust Embedded matrix channel], lots of people who contribute or work on embedded software
  hang out there. Including for example the people who wrote the `microbit` BSP, the `nrf52-hal`, `embedded-hal` etc.-->
  
- [Rust Embedded matrix channel]に参加するのもいいかもしれません。組込みソフトウェアに貢献している開発者たちが集まっています。`microbit`のBSP、`nrf52-hal`、`embedded-hal`などを書いた人たちです。

[Rust Embedded matrix channel]: https://matrix.to/#/#rust-embedded:matrix.org

<!--- If you are looking for a general overview of what is available in Rust Embedded right now check out the [Awesome Rust Embedded]
  list-->
  
- 組込みRust界の現状について一般的な情報がほしいということであれば、[Awesome Rust Embedded]リストがおすすめです。

[Awesome Rust Embedded]: https://github.com/rust-embedded/awesome-embedded-rust/

<!--- You could check out [Real-Time Interrupt-driven Concurrency]. A very efficient preemptive multitasking framework
  that supports task prioritization and dead lock free execution.-->
  
- [Real-Time Interrupt-driven Concurrency]を試してみてもいいでしょう。非常に効率的なプリエンプティブ・マルチタスク・フレームワークです。タスクの優先順位づけとデッドロックのない実行をサポートしてくれます。

[Real-Time Interrupt-driven Concurrency]: https://rtic.rs

<!--- You could check out more abstractions of the [`embedded-hal`] project and maybe even try and write your own
  platform agnostic driver based on it.-->
  
- [`embedded-hal`]による抽象化をさらに深く学ぶのもいいかもしれません。また、それを利用してプラットフォームに依存しないドライバをあなた自身で書いてみるのもいいでしょう。

[`embedded-hal`]: https://github.com/rust-embedded/embedded-hal

<!--- You could try running Rust on a different development board. The easiest way to get started is to
  use the [`cortex-m-quickstart`] Cargo project template.-->
  
- Rustを別の開発ボードで走らせてもいいでしょう。一番簡単な始め方は、[`cortex-m-quickstart`]というプロジェクトテンプレートを使うことです。

[`cortex-m-quickstart`]: https://docs.rs/cortex-m-quickstart/0.3.1/cortex_m_quickstart/

<!--- You could try out [this motion sensors demo][madgwick]. Details about the implementation and
  source code are available in [this blog post][wd-1-2].-->
  
- [このモーションセンサのデモ][madgwick]を試すこともできます。実装の詳細とソースコードは[このブログ記事][wd-1-2]にあります。

[madgwick]: https://mobile.twitter.com/japaricious/status/962770003325005824
[wd-1-2]: http://blog.japaric.io/wd-1-2-l3gd20-lsm303dlhc-madgwick/

<!--- You could check out [this blog post][brave-new-io] which describes how Rust type system can
  prevent bugs in I/O configuration.-->
  
- Rustの型システムがいかにI/O設定におけるバグを防ぐかについて、[このブログ記事][brave-new-io]が解説してくれています。

[brave-new-io]: http://blog.japaric.io/brave-new-io/

<!--- You could check out [japaric's blog] for miscellaneous topics about embedded development with Rust.-->

- [japaric氏のブログ]は組込みRustについて幅広いトピックを扱っています。

<!--[japaric's blog]: http://blog.japaric.io-->

[japaric氏のブログ]: http://blog.japaric.io


<!--- You could join the [Weekly driver initiative] and help us write generic drivers on top of the
  `embedded-hal` traits and that work for all sorts of platforms (ARM Cortex-M, AVR, MSP430, RISCV,
  etc.)-->
  
- [Weekly driver initiative]に参加するのもいいでしょう。`embedded-hal`トレイトを利用して、多くのプラットフォーム（ARM Cortex-M, AVR, MSP430, RISCVなど）で動作する汎用性の高いドライバを開発してみませんか。

[Weekly driver initiative]: https://github.com/rust-lang-nursery/embedded-wg/issues/39
