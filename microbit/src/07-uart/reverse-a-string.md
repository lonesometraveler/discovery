<!-- # Reverse a string -->

# 文字列を反転させる

<!-- Alright, next let's make the server more interesting by having it respond to the client with the
reverse of the text that they sent. The server will respond to the client every time they press the
ENTER key. Each server response will be in a new line. -->

では次に、サーバーをもっとおもしろくしてみましょう。クライアントが送るテキストを反転させてから返信してください。サーバーは、ENTERキーが押されるたびに返信することとします。返信はそのつど新しい行となります。

<!-- This time you'll need a buffer; you can use [`heapless::Vec`]. Here's the starter code: -->

今回はバッファが必要ですので、[`heapless::Vec`]を使ってください。これがスターターコードです：

[`heapless::Vec`]: https://docs.rs/heapless/0.7.7/heapless/struct.Vec.html

``` rust
#![no_main]
#![no_std]

use cortex_m_rt::entry;
use core::fmt::Write;
use heapless::Vec;
use rtt_target::rtt_init_print;
use panic_rtt_target as _;

#[cfg(feature = "v1")]
use microbit::{
    hal::prelude::*,
    hal::uart,
    hal::uart::{Baudrate, Parity},
};

#[cfg(feature = "v2")]
use microbit::{
    hal::prelude::*,
    hal::uarte,
    hal::uarte::{Baudrate, Parity},
};

#[cfg(feature = "v2")]
mod serial_setup;
#[cfg(feature = "v2")]
use serial_setup::UartePort;

#[entry]
fn main() -> ! {
    rtt_init_print!();
    let board = microbit::Board::take().unwrap();

    #[cfg(feature = "v1")]
    let mut serial = {
        uart::Uart::new(
            board.UART0,
            board.uart.into(),
            Parity::EXCLUDED,
            Baudrate::BAUD115200,
        )
    };

    #[cfg(feature = "v2")]
    let mut serial = {
        let serial = uarte::Uarte::new(
            board.UARTE0,
            board.uart.into(),
            Parity::EXCLUDED,
            Baudrate::BAUD115200,
        );
        UartePort::new(serial)
    };

    // 32バイト容量のバッファ
    let mut buffer: Vec<u8, 32> = Vec::new();

    loop {
        buffer.clear();

        // TODO クライアントからのリクエストを受信してください。それぞれのリクエストはENTERで終わります。
        // 注 `buffer.push`は`Result`を返します。エラーメッセージを返信することで、エラーを処理してください。

        // TODO 反転させた文字列を返信してください。
    }
}
```
