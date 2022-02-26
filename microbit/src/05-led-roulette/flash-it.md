<!-- # Flash it -->

# フラッシュに書き込む

<!--
Flashing is the process of moving our program into the microcontroller's (persistent) memory. Once
flashed, the microcontroller will execute the flashed program every time it is powered on.
-->

フラッシュに書き込むということは、マイクロコントローラの（永続）メモリにプログラムを移動するプロセスです。
一度フラッシュに書き込むと、電源がオンになるたびにマイクロコントローラはフラッシュに書かれたプログラムを実行します。

<!--
In this case, our `led-roulette` program will be the *only* program in the microcontroller memory.
By this I mean that there's nothing else running on the microcontroller: no OS, no "daemon",
nothing. `led-roulette` has full control over the device.
-->

今回の場合、`led-roulette`はマイクロコントローラのメモリに存在する*唯一の*プログラムです。
これはこのマイクロコントローラ上で他に何も動いていないことを意味しています。
OSも「デーモン」も動いていません。
`led-roulette`はデバイスの全ての制御を握っています。

<!--
Flashing the binary itself is quite simple thanks to `cargo embed`.
-->

`cargo embed`のおかげでバイナリをフラッシュに書き込むことは極めて簡単です。

<!--
Before executing that command though, let's look into what it actually does. If you look at the side of your micro:bit
with the USB connector facing upwards you will notice, that there are actually 2 black squares on there
(on the micro:bit v2 there is a third and biggest one, which is a speaker), one is our MCU
we already talked about but what purpose does the other one serve? The other chip has 3 main purposes:
-->

コマンドを実行する前に、このコマンドが何をするのか見てみましょう。
micro:bitのUSBコネクタがある側を見てみると、2つの黒い四角があることに気づくでしょう（micro:bit2では3つ目の一番大きなスピーカーもあります）。
1つは既に話したとおりMCUです。
それではもう1つは何をするものなのでしょうか？
このもう1つのチップは主に3つの役割を果たします。

<!--
1. Provide power from the USB connector to our MCU
2. Provide a serial to USB bridge for our MCU (we will look into that in a later chapter)
3. Being a programmer/debugger (this is the relevant purpose for now)
-->

1. MCUにUSBコネクタからの電源を供給する
2. MCUのシリアルとUSBのブリッジ機能を提供する（後の章で説明します）
3. プログラマー/デバッガーになる（これが今のお目当てです）

<!--
Basically this chip acts as a bridge between our computer (to which it is connected via USB) and the MCU (to which it is
connected via traces and communicates with using the SWD protocol). This bridge enables us to flash new binaries on to
the MCU, inspect its state via a debugger and other things.
-->

基本的にこのチップは私たちのコンピュータ（USBで接続されている）とMCU（トレースで接続されていてSWDプロトコルで通信する）とのブリッジです。
このブリッジのおかげでMCUのフラッシュに新しいバイナリを書き込めるし、デバッガーを使ってMCUの状態を調査したりできます。

<!--
So lets flash it!
-->

ファラッシュに書き込んでみましょう！

```console
# For micro:bit v2
$ cargo embed --features v2 --target thumbv7em-none-eabihf
  (...)
     Erasing sectors ✔ [00:00:00] [####################################################################################################################################################]  2.00KiB/ 2.00KiB @  4.21KiB/s (eta 0s )
 Programming pages   ✔ [00:00:00] [####################################################################################################################################################]  2.00KiB/ 2.00KiB @  2.71KiB/s (eta 0s )
    Finished flashing in 0.608s

# For micro:bit v1
$ cargo embed --features v1 --target thumbv6m-none-eabi
  (...)
     Erasing sectors ✔ [00:00:00] [####################################################################################################################################################]  2.00KiB/ 2.00KiB @  4.14KiB/s (eta 0s )
 Programming pages   ✔ [00:00:00] [####################################################################################################################################################]  2.00KiB/ 2.00KiB @  2.69KiB/s (eta 0s )
    Finished flashing in 0.614s
```

<!--
You will notice that `cargo-embed` blocks after outputting the last line, this is intended and you should not close it
since we need it in this state for the next step: debugging it! Furthermore, you will have noticed that the `cargo build`
and `cargo embed` are actually passed the same flags, this is because `cargo embed` actually executes the build and then
flashes the resulting binary on to the chip, hence you can leave out the `cargo build` step in the future if you
want to flash your code right away.
-->

`cargo-embed`が最後の1行を出力したあと、（訳注：実行が終了せずにコンソールを）ブロックしていることに気づくでしょう。
これは意図通り動いており、閉じてはいけません。
次のステップでデバッグするためにこのままの状態にしておく必要があります。
さらに、`cargo build`と`cargo embed`とに同じフラグが渡されていることにも気づくでしょう。
これは`cargo embed`がビルドを実行して、そのあとチップにビルド結果のバイナリを書き込んでいるためです。
そのため、新しいコードをすぐにフラッシュに書き込みたいときは`cargo build`を省略できます。
