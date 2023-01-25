<!-- # Using a driver -->

# ドライバを使う

<!-- As we already discussed in chapter 5 `embedded-hal` provides abstractions
which can be used to write platform independent code that can interact with
hardware. In fact all the methods we have used to interact with hardware
in chapter 7 and up until now in chapter 8 were from traits, defined by `embedded-hal`.
Now we'll make actual use of the traits `embedded-hal` provides for the first time. -->

５章ですでに触れたとおり、`embedded-hal`が提供する抽象化のおかげで、プラットフォームに依存することなくハードウェアとやりとりするコードを書くことができます。実際に、７章と８章でこれまでに使用したメソッドはすべて`embedded-hal`が定義するトレイトから来ていました。ここで私たちも初めて`embedded-hal`のトレイトを利用してみましょう。

<!-- It would be pointless to implement a driver for our LSM303AGR for every platform
embedded Rust supports (and new ones that might eventually pop up). To avoid this a driver
can be written that consumes generic types that implement `embedded-hal` traits in order to provide
a platform agnostic version of a driver. Luckily for us this has already been done in the
[`lsm303agr`] crate. Hence reading the actual accelerometer and magnetometer values will now
be basically a plug and play experience (plus reading a bit of documentation). In fact the `crates.io`
page already provides us with everything we need to know in order to read accelerometer data but using a Raspberry Pi. We'll
just have to adapt it to our chip: -->

Rustがサポートする（そして将来サポートするであろう）すべての組込みプラットフォームに別々のLSM303AGR用ドライバを開発しなくてはならないとしたら、それはまったくもって無駄なことです。それを避けるために、`embedded-hal`トレイトを実装したジェネリック型を消費するコードを書き、プラットフォームに依存しないドライバを作ります。幸運なことに、その作業はすでに[`lsm303agr`] クレート内でなされています。ですから加速度計、磁力計の値を読み取るのは、（少しばかりドキュメンテーションを読むことを除けば）基本的にプラグアンドプレイです。実際に、`crates.io`のページを見れば、Raspberry Pi向けにではありますが、加速度計のデータを読み取るのに必要な情報はすべて書いてあります。あとはそれを私たちが使うチップ向けに修正するだけです。

[`lsm303agr`]: https://crates.io/crates/lsm303agr

```rust
use linux_embedded_hal::I2cdev;
use lsm303agr::{AccelOutputDataRate, Lsm303agr};

fn main() {
    let dev = I2cdev::new("/dev/i2c-1").unwrap();
    let mut sensor = Lsm303agr::new_with_i2c(dev);
    sensor.init().unwrap();
    sensor.set_accel_odr(AccelOutputDataRate::Hz50).unwrap();
    loop {
        if sensor.accel_status().unwrap().xyz_new_data {
            let data = sensor.accel_data().unwrap();
            println!("Acceleration: x {} y {} z {}", data.x, data.y, data.z);
        }
    }
}
```

<!-- Because we already know how to create an instance of an object that implements
the [`embedded_hal::blocking::i2c`] traits from the [previous page](read-a-single-register.md), this is quite trivial: -->

[`embedded_hal::blocking::i2c`]トレイトを実装したオブジェクトのインスタンス生成方法は[前のページ](read-a-single-register.md)で確認しましたから、修正は簡単なはずです。

[`embedded_hal::blocking::i2c`]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/blocking/i2c/index.html

```rust
#![deny(unsafe_code)]
#![no_main]
#![no_std]

use cortex_m_rt::entry;
use rtt_target::{rtt_init_print, rprintln};
use panic_rtt_target as _;

#[cfg(feature = "v1")]
use microbit::{
    hal::twi,
    pac::twi0::frequency::FREQUENCY_A,
};

#[cfg(feature = "v2")]
use microbit::{
    hal::twim,
    pac::twim0::frequency::FREQUENCY_A,
};

use lsm303agr::{
    AccelOutputDataRate, Lsm303agr,
};

#[entry]
fn main() -> ! {
    rtt_init_print!();
    let board = microbit::Board::take().unwrap();


    #[cfg(feature = "v1")]
    let i2c = { twi::Twi::new(board.TWI0, board.i2c.into(), FREQUENCY_A::K100) };

    #[cfg(feature = "v2")]
    let i2c = { twim::Twim::new(board.TWIM0, board.i2c_internal.into(), FREQUENCY_A::K100) };

    // ドキュメンテーションからのコード
    let mut sensor = Lsm303agr::new_with_i2c(i2c);
    sensor.init().unwrap();
    sensor.set_accel_odr(AccelOutputDataRate::Hz50).unwrap();
    loop {
        if sensor.accel_status().unwrap().xyz_new_data {
            let data = sensor.accel_data().unwrap();
            // printの代わりにRTTを使用します
            rprintln!("Acceleration: x {} y {} z {}", data.x, data.y, data.z);
        }
    }
}
```

<!-- Just like the last snippet you should just be able to try this out like this: -->

前のページでやったように、以下のコマンドでこのプログラムを試すことができます。

```console
# For micro:bit v2
$ cargo embed --features v2 --target thumbv7em-none-eabihf

# For micro:bit v1
$ cargo embed --features v1 --target thumbv6m-none-eabi
```

<!-- Furthermore if you (physically) move around your micro:bit a little you should see the
acceleration numbers that are being printed change. -->

さらに、もしmicro:bitを（物理的に）動かしてみれば、画面にプリントされる加速度計の値が変化するのを確認できるでしょう。
