<!-- # My solution -->

# 解答例

``` rust
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
    AccelScale, AccelOutputDataRate, Lsm303agr,
};

use microbit::hal::timer::Timer;
use microbit::hal::prelude::*;
use nb::Error;

#[entry]
fn main() -> ! {
    const THRESHOLD: f32 = 0.5;

    rtt_init_print!();
    let board = microbit::Board::take().unwrap();

    #[cfg(feature = "v1")]
    let i2c = { twi::Twi::new(board.TWI0, board.i2c.into(), FREQUENCY_A::K100) };

    #[cfg(feature = "v2")]
    let i2c = { twim::Twim::new(board.TWIM0, board.i2c_internal.into(), FREQUENCY_A::K100) };

    let mut countdown = Timer::new(board.TIMER0);
    let mut delay = Timer::new(board.TIMER1);
    let mut sensor = Lsm303agr::new_with_i2c(i2c);
    sensor.init().unwrap();
    sensor.set_accel_odr(AccelOutputDataRate::Hz50).unwrap();
    // 16Gまで計測できるようにセンサの設定を変更。
    // 人のパンチは案外速いものです。
    sensor.set_accel_scale(AccelScale::G16).unwrap();

    let mut max_g = 0.;
    let mut measuring = false;

    loop {
        while !sensor.accel_status().unwrap().xyz_new_data {}
        // X軸の加速度をg単位で取得
        let g_x = sensor.accel_data().unwrap().x as f32 / 1000.0;

        if measuring {
            // contdownタイマのステータスをチェック
            match countdown.wait() {
                // countdownはまだ終わっていない
                Err(Error::WouldBlock) => {
                    if g_x > max_g {
                        max_g = g_x;
                    }
                },
                // countdown終了
                Ok(_) => {
                    // 最大加速度を報告
                    rprintln!("Max acceleration: {}g", max_g);

                    // リセット
                    max_g = 0.;
                    measuring = false;
                },
                // nrf52とnrf51のHALはエラー型としてVoidを返します。
                // VoidはEmpty型なので、ここにたどり着くことはありません。
                Err(Error::Other(_)) => {
                    unreachable!()
                }
            }
        } else {
            // 加速度がしきい値を超えれば測定を開始
            if g_x > THRESHOLD {
                rprintln!("START!");

                measuring = true;
                max_g = g_x;
                // ドキュメンテーションによると、タイマは1Mhzで動作します。
                // よって、１秒待つためには1_000_000ティック必要です。
                countdown.start(1_000_000_u32);
            }
        }
        delay.delay_ms(20_u8);
    }
}
```
