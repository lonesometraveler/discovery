<!-- # Take 1 -->

# 課題１

<!-- What's the simplest way in which we can implement the LED compass, even if it's not perfect? -->

完璧でなくてもいいので、一番簡単にLEDコンパスを実装するとしたらどうするでしょうか？

<!-- For starters, we'd only care about the X and Y components of the magnetic field because when you
look at a compass you always hold it in horizontal position and thus the compass is in the XY plane. -->

まず、XとYだけに注目するべきでしょう。コンパスを見るときはいつも水平に持つので、コンパスはXY平面にあるといえるからです。

<p align="center">
<img class="white_bg" title="Quadrants" src="../assets/quadrants.png">
</p>

<!-- If we only looked at the signs of the X and Y components we could determine to which quadrant the
magnetic field belongs to. Now the question of course is which direction (north, north-east, etc.)
do the 4 quadrants represent. In order to figure this out we can just rotate the micro:bit and observe
how the quadrant changes whenever we point in another direction. -->

XとYの値だけに注目すれば、磁場がどの象限に属しているのかわかります。問題は、それぞれの象限がどの方角を指し示しているか、です。それを知るためには、micro:bitを回転させ、違う方角を指したときに象限がどう変化するかを見ればいいです。

<!-- After experimenting a bit we can find out that if we point the micro:bit in e.g. north-east direction,
both the X and the Y component are always positive. Based on this information you should be able to
figure out which direction the other quadrants represent. -->

しばらく実験すると、micro:bitを北東に向けるとXとYはつねに正だ、といったことがわかるでしょう。この情報を元に、他の象限がどの方角を表しているかわかるはずです。

<!-- Once you figured out the relation between quadrant and direction you should be able to
complete the template from below. -->

象限と方角の関係性さえわかれば、下のテンプレートを完成させることができるはずです。

```rust
#![deny(unsafe_code)]
#![no_main]
#![no_std]

use cortex_m_rt::entry;
use panic_rtt_target as _;
use rtt_target::{rprintln, rtt_init_print};

mod calibration;
use crate::calibration::calc_calibration;
use crate::calibration::calibrated_measurement;

mod led;
use led::Direction;

use microbit::{display::blocking::Display, hal::Timer};

#[cfg(feature = "v1")]
use microbit::{hal::twi, pac::twi0::frequency::FREQUENCY_A};

#[cfg(feature = "v2")]
use microbit::{hal::twim, pac::twim0::frequency::FREQUENCY_A};

use lsm303agr::{AccelOutputDataRate, Lsm303agr, MagOutputDataRate};

#[entry]
fn main() -> ! {
    rtt_init_print!();
    let board = microbit::Board::take().unwrap();

    #[cfg(feature = "v1")]
    let i2c = { twi::Twi::new(board.TWI0, board.i2c.into(), FREQUENCY_A::K100) };

    #[cfg(feature = "v2")]
    let i2c = { twim::Twim::new(board.TWIM0, board.i2c_internal.into(), FREQUENCY_A::K100) };

    let mut timer = Timer::new(board.TIMER0);
    let mut display = Display::new(board.display_pins);

    let mut sensor = Lsm303agr::new_with_i2c(i2c);
    sensor.init().unwrap();
    sensor.set_mag_odr(MagOutputDataRate::Hz10).unwrap();
    sensor.set_accel_odr(AccelOutputDataRate::Hz10).unwrap();
    let mut sensor = sensor.into_mag_continuous().ok().unwrap();

    let calibration = calc_calibration(&mut sensor, &mut display, &mut timer);
    rprintln!("Calibration: {:?}", calibration);
    rprintln!("Calibration done, entering busy loop");
    loop {
        while !sensor.mag_status().unwrap().xyz_new_data {}
        let mut data = sensor.mag_data().unwrap();
        data = calibrated_measurement(data, &calibration);

        let dir = match (data.x > 0, data.y > 0) {
            // 第I象限
            (true, true) => Direction::NorthEast,
            // 第II象限 ???
            (false, true) => panic!("TODO"),
            // 第III象限 ???
            (false, false) => panic!("TODO"),
            // 第IV象限 ???
            (true, false) => panic!("TODO"),
        };
			
        // ledモジュール（src/led.rs）を利用して、方角をLED矢印に変換してください。
        // それから５章で使ったLEDディスプレイ関数を使い、矢印をLEDマトリクス上に表示してください。
    }
}
```
