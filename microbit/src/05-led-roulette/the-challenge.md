<!--# The challenge-->

# 課題

<!--You are now well armed to face a challenge! Your task will be to implement the application I showed
you at the beginning of this chapter.-->

さあ課題に挑戦する準備はできました！　この章の最初にお見せしたこのアプリケーションを実装してください。

<p align="center">
<video src="../assets/roulette_fast.mp4" loop autoplay>
</p>

<!--If you can't exactly see what's happening here it is in a much slower version:-->

もしもなにが起きているのかわかりづらければ、動きを遅くしたこちらを見てください。

<p align="center">
<video src="../assets/roulette_slow.mp4" loop autoplay>
</p>

<!--Since working with the LED pins separately is quite annoying
(especially if you have to use basically all of them like here)
you can use the display API provided by the BSP. It works like this:-->

LEDを駆動するピンをそれぞれ個別に操作するのは（とくに全部のLEDとなると）めんどうですから、BSPが提供するディスプレイAPIを使うといいです。こういうふうになります。

```rust
#![deny(unsafe_code)]
#![no_main]
#![no_std]

use cortex_m_rt::entry;
use rtt_target::rtt_init_print;
use panic_rtt_target as _;
use microbit::{
    board::Board,
    display::blocking::Display,
    hal::{prelude::*, Timer},
};

#[entry]
fn main() -> ! {
    rtt_init_print!();

    let board = Board::take().unwrap();
    let mut timer = Timer::new(board.TIMER0);
    let mut display = Display::new(board.display_pins);
    let light_it_all = [
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
        [1, 1, 1, 1, 1],
    ];

    loop {
        // light_it_allを1000ミリ秒表示
        display.show(&mut timer, light_it_all, 1000);
        // ディスプレイをクリアする
        display.clear();
        timer.delay_ms(1000_u32);
    }
}
```

<!--Equipped with this API your task basically boils down to just having
to calculate the proper image matrix and passing it into the BSP.-->

このAPIがあれば、あとは適切な画像マトリクスを計算して、それをBSPに渡してやるだけです。
