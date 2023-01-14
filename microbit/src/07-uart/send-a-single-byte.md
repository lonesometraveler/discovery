<!-- # Send a single byte -->

# １バイト送信する

<!-- Our first task will be to send a single byte from the microcontroller to the computer over the serial
connection. -->

最初のタスクは、シリアル通信でマイクロコントローラからコンピュータに１バイト送ることです。

<!-- In order to do that we will use the following snippet (this one is already in `07-uart/src/main.rs`): -->

そのために、以下のコードを使いましょう。（`07-uart/src/main.rs`中にあるものです。）

``` rust
{{#include src/main.rs}}
```

<!-- The most prevalent new thing here is obviously the `cfg` directives to conditionally include/exclude
parts of the code. This is mostly just because we want to work with a regular UART for the micro:bit v1
and with the UARTE for micro:bit v2. -->

まず目新しいものといえば、`cfg`ディレクティブでしょう。これは条件によって特定のコードセクションをソースに含めたり除外したりするためのもので、ここではmicro:bit v1用にUART、micro:bit v2用にはUARTEを使用するよう指定しています。

<!-- You will also have noticed that this is the first time we are including some code that is not from a library,
namely the `serial_setup` module. Its only purpose is to provide a nice wrapper around the UARTE
so we can use it the exact same way as the UART via the [`embedded_hal::serial`] traits. If you want, you can
check out what exactly the module does, but it is not required to understand this chapter in general. -->

また、ここで初めてライブラリ外のコードを取り込んでいることにもお気づきでしょう。`serial_setup`モジュールのことです。UARTEのラッパであるこのモジュールを使うことで、UARTEもUARTとまったく同じように[`embedded_hal::serial`]トレイト経由で扱うことができます。この章の理解には必要ありませんが、もし興味があればモジュールの設計をのぞいてみてください。

[`embedded_hal::serial`]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/serial/index.html

<!-- Apart from those differences, the initialization procedures for the UART and the UARTE are quite similar so we'll
discuss the initialization of just UARTE. The UARTE is initialized with this piece of code: -->

これらの違いを除けば、UARTとUARTEの初期化手続きはよく似ています。ですからここではUARTEの初期化についてだけ解説します。UARTEは以下のコードで初期化します。

```rs
uarte::Uarte::new(
    board.UARTE0,
    board.uart.into(),
    Parity::EXCLUDED,
    Baudrate::BAUD115200,
);
```
<!-- This function takes ownership of the UARTE peripheral representation in Rust (`board.UARTE0`) and the TX/RX pins
on the board (`board.uart.into()`) so nobody else can mess with either the UARTE peripheral or our pins while
we are using them. After that we pass two configuration options to the constructor: the baudrate (that one should be
familiar) as well as an option called "parity". Parity is a way to allow serial communication lines to check whether
the data they received was corrupted during transmission. We don't want to use that here so we simply exclude it.
Then we wrap it up in the `UartePort` type so we can use it the same way as the micro:bit v1's `serial`. -->

この関数はRustで表現されたUARTEペリフェラル（`board.UARTE0`）、ならびにTX/RXピン(`board.uart.into()`)の所有権を取得します。こうすることで、私たち以外の誰もUARTEとピンを使うことができなくなります。次に、ふたつの設定オプションをコンストラクタに渡します。ボーレート（`Baudrate`）とパリティ（`Parity`）です。パリティはシリアル通信ラインに受信したデータが破損していないか確認することを可能にするオプションです。ここでは使いませんので、除外（`EXCLUDED`）しておきましょう。最後に`UartePort`型で包んでやります。こうすることで、micro:bit v1の `serial`と同じように扱うことが可能になります。

<!-- After the initialization, we send our `X` via the newly created uart instance. The `block!` macro here is the `nb::block!`
macro. `nb` is a (quoting from its description) "Minimal and reusable non-blocking I/O layer". It allows us to write
code that can conduct hardware operations in the background while we go and do other work (non-blocking). However,
in this and many other cases we have no interest in doing some other work so we just call `block!` which will wait until
the I/O operation is done and has either succeeded or failed and then continue execution normally. -->

初期化できたら、今作ったばかりのUARTインスタンスで`X`を送ります。ここに出てくる`block!`マクロは、`nb::block!`マクロです。`nb`は、「最小限かつ再利用可能なノンブロッキングI/O層」（公式ドキュメントからの引用）です。これによって、バックグラウンドでハードウェア操作を行うあいだに別タスクを処理（ノンブロッキング）することができます。ですが、このケースでは平行して別の処理はしないので、ここではただ`block!` を呼び、I/O処理の成功・失敗を待ってからプログラムの実行を続けることにします。

<!-- Last but not least, we `flush()` the serial port. This is because an implementor of the `embedded-hal::serial` traits may
decide to buffer output until it has received a certain number of bytes to send (this is the case with the UARTE implementation).
Calling `flush()` forces it to write the bytes it currently has right now instead of waiting for more. -->

最後に、シリアルポートを`flush()`します。なぜかというと、`embedded-hal::serial`トレイトの実装によっては、決まったバイト数を受信するまで出力をバッファにためておく設計となっていることがあるからです。（実際にUARTEはそのように実装されています。）`flush()`を呼ぶことで、さらなる受信を待つことなく、これまでに受信したバイトを強制的に出力させることができるのです。

<!-- ## Testing it -->

## テストしましょう

<!-- Before flashing this you should make sure to start your minicom/PuTTY as the data we receive via our serial
communication is not backed up or anything, we have to view it live. Once your serial monitor is up you can
flash the program just like in chapter 5: -->

プログラムを書き込む前に、忘れずにminicom/PuTTYをスタートしてください。シリアル通信で受信するデータは、どこかに保存されるわけではなく、リアルタイムに観察する必要があるからです。シリアルモニタが立ち上がったら、５章でしたようにプログラムを書き込みましょう。

```
# For micro:bit v2
$ cargo embed --features v2 --target thumbv7em-none-eabihf
  (...)

# For micro:bit v1
$ cargo embed --features v1 --target thumbv6m-none-eabi
```

<!-- And after the flashing is finished, you should see the character `X` show up on your minicom/PuTTY terminal, congrats! -->

書き込みが終わると、`X`の文字がminicom/PuTTYのターミナルに出るはずです。おめでとうございます！
