# UART

<!-- The microcontroller has a peripheral called UART, which stands for Universal
Asynchronous Receiver/Transmitter. This peripheral can be configured to work with
several communication protocols like the serial communication protocol. -->

マイクロコントローラには、UART（Universal
Asynchronous Receiver/Transmitter）と呼ばれるペリフェラルがあります。このペリフェラルは、シリアル通信など、いくつかの通信プロトコルで動作するように設定できます。


<!-- Throughout this chapter, we'll use serial communication to exchange information between the
microcontroller and your computer. -->

<!-- > **NOTE** that on the micro:bit v2 we will use the so called UARTE peripheral which behaves
> just like a regular UART, except that the HAL has to talk to it differently.
> However, this will of course not be our concern. -->

この章では、シリアル通信を使ってマイクロコントローラとコンピュータ間で情報のやり取りをします。

> **注** micro:bit v2ではUARTEと呼ばれるペリフェラルを使いますが、普通のUARTと同じように動作します。HALとペリフェラルのやり取りに違いがあるのですが、ここではそれを気にする必要はまったくありません。

<!-- ## Setup -->

## セットアップ

<!-- As always from now on you will have to modify the `Embed.toml` to match your micro:bit version: -->

これまでのように、`Embed.toml`をお使いのmicro:bitのバージョンに合わせて修正してください。

```toml
{{#include Embed.toml}}
```
