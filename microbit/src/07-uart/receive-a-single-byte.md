<!-- # Receive a single byte -->

# １バイト受信する

<!-- So far we can send data from the microcontroller to your computer. It's time to try the opposite: receiving
data from your computer. Luckily `embedded-hal` has again got us covered with this one: -->

これまでマイクロコントローラからコンピュータにデータを送信してきました。今度はその逆、コンピュータからの受信を試してみましょう。幸いなことに、これもまた`embedded-hal`のおかげで簡単です。

``` rust
#![no_main]
#![no_std]

use cortex_m_rt::entry;
use rtt_target::{rtt_init_print, rprintln};
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

    loop {
        let byte = nb::block!(serial.read()).unwrap();
        rprintln!("{}", byte);
    }
}
```

<!-- The only part that changed, compared to our send byte program, is the loop
at the end of `main()`. Here we use the `read()` function, provided by `embedded-hal`,
in order to wait until a byte is available and read it. Then we print that byte
into our RTT debugging console to see whether stuff is actually arriving. -->

１バイト送信プログラムからの唯一の変更点は、`main()`の終わりにあるループです。ここで`embedded-hal`が提供する`read()`関数を使い、１バイト読み込むのを待っています。次にそのバイトをRTTデバッグコンソールにプリントして、データが本当に来ているか確認しています。

<!-- Note that if you flash this program and start typing characters inside `minicom` to
send them to your microcontroller you'll only be able to see numbers inside your
RTT console since we are not converting the `u8` we received into an actual `char`.
Since the conversion from `u8` to `char` is quite simple, I'll leave this task to
you if you really do want to see the characters inside the RTT console. -->

注意点があります。このプログラムを書き込んで、`minicom`に文字をタイプするとき、RTTコンソールには数字だけが表示されます。というのも、このプログラムは受信した`u8`を実際の文字である`char`に変換しないからです。`u8`から`char`への変換はとても簡単です。どうしてもRTTコンソールで文字を確認したければ、ご自分で実装してみてください。
