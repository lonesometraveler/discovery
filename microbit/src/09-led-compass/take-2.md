<!-- # Take 2 -->

# 課題２

<!-- This time, we'll use math to get the precise angle that the magnetic field forms with the X and Y
axes of the magnetometer. -->

今回は、磁力計のX、Y軸から、数学を使って磁場の正確な角度を求めます。

<!-- We'll use the `atan2` function. This function returns an angle in the `-PI` to `PI` range. The
graphic below shows how this angle is measured: -->

`atan2`関数を使います。この関数は、`-PI`から `PI`の範囲で角度を返します。角度がどのように測定されているのかは下図を見てください。

<p align="center">
<img class="white_bg" title="atan2" src="https://upload.wikimedia.org/wikipedia/commons/0/03/Atan2_60.svg">
</p>

<!-- Although not explicitly shown in this graph the X axis points to the right and the Y axis points up. -->

この図では明確に示されていませんが、X軸は右に、Y軸は上に向いています。

<!-- Here's the starter code. `theta`, in radians, has already been computed. You need to pick which LED
to turn on based on the value of `theta`. -->

これがスターターコードです。ラジアンの`theta`は計算済みです。`theta`の値をもとに、どのLEDを点灯するか決めてください。

```rs
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
use crate::led::Direction;
use crate::led::direction_to_led;

// You'll find this useful ;-)
use core::f32::consts::PI;
use libm::atan2f;

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

        // atan2はcoreに入っていないので、libmのatan2fを使います。
        let theta = atan2f(data.x as f32, data.y as f32);

        // thetaの値から、指す方角を決めてください。
        let dir = Direction::NorthEast;

        display.show(&mut timer, direction_to_led(dir), 100);
    }
}
```

<!-- Suggestions/tips: -->

ヒントとアドバイス:

<!-- 
- A whole circle rotation equals 360 degrees.
- `PI` radians is equivalent to 180 degrees.
- If `theta` was zero, which direction are you pointing at?
- If `theta` was, instead, very close to zero, which direction are you pointing at?
- If `theta` kept increasing, at what value would you change the direction
- -->

- 円全体の回転角は、360度です。
- `PI`ラジアンは180度です。
- `theta`がゼロのとき、どの方角を指しますか？
- `theta`がゼロにとても近いとき、どの方角を指しますか？
- `theta`が増え続けるとき、どの値になったら方角を変えますか？
