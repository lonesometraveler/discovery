<!-- # Calibration -->

# キャリブレーション

<!-- One very important thing to do before using a sensor and trying to develop
an application using it is verifying that it's output is actually correct.
If this does not happen to be the case we need to calibrate the sensor
(alternatively it could also be broken but that's rather unlikely in this case). -->

センサを使ってアプリケーションを作る前に、まずなによりも大切なことがあります。センサの出力が正しいことを確認することです。もし正しくないようなら、センサのキャリブレーションが必要です。（可能性は低いですが、故障しているということもあります。）

<!-- In my case on two different micro:bit's the magnetometer, without calibration,
was quite a bit off of what it is supposed to measure. Hence for the purposes
of this chapter we will just assume that the sensor has to be calibrated. -->

筆者がふたつのmicro:bitの磁力計をキャリブレーションせずに試してみたところ、想定される測定値からずいぶん外れていることがわかりました。ですからこの章では、センサはキャリブレーションされるべきものだとします。

<!-- The calibration involves quite a bit of math (matrices) so we won't cover it here but this
[Design Note] describes the procedure if you are interested. -->

キャリブレーションにはかなりの数学（行列）が必要となるので、ここでは詳細は説明しません。もしも処理の仕方に興味があれば、[アプリケーションノート]に解説があります。

<!-- [Design Note]: https://www.st.com/resource/en/design_tip/dt0103-compensating-for-magnetometer-installation-error-and-hardiron-effects-using-accelerometerassisted-2d-calibration-stmicroelectronics.pdf -->

[アプリケーションノート]: https://www.st.com/resource/en/design_tip/dt0103-compensating-for-magnetometer-installation-error-and-hardiron-effects-using-accelerometerassisted-2d-calibration-stmicroelectronics.pdf

<!-- Luckily for us though the group that built the original software for the
micro:bit already implemented a calibration mechanism in C++ over [here]. -->

ただ幸いにも、キャリブレーションの仕組みはmicro:bitのソフトウェア開発チームがすでにC++で実装してくれています。これがその[コード]です。

<!-- [here]: https://github.com/lancaster-university/codal-microbit-v2/blob/006abf5566774fbcf674c0c7df27e8a9d20013de/source/MicroBitCompassCalibrator.cpp -->

[コード]: https://github.com/lancaster-university/codal-microbit-v2/blob/006abf5566774fbcf674c0c7df27e8a9d20013de/source/MicroBitCompassCalibrator.cpp

<!-- You can find a translation of it to Rust in `src/calibration.rs`. The usage
is demonstrated in the default `src/main.rs` file. The way the calibration
works is illustrated in this video: -->

このコードのRustへの翻訳は、`src/calibration.rs`を見てください。使用例は`src/main.rs`で確認できます。実際のキャリブレーションの様子は、このビデオを見てください。

<p align="center">
<video src="https://video.microbit.org/support/compass+calibration.mp4" loop autoplay>
</p>

<!-- You have to basically tilt the micro:bit until all the LEDs on the LED matrix light up. -->

要は、LEDマトリクス上のすべてのLEDが点灯するまでmicro:bitを傾ければいいのです。

<!-- If you do not want to play the game every time you restart your application during development
feel free to modify the `src/main.rs` template to just use the same static calibration
once you got the first one. -->

アプリケーションをリスタートするたびにこの作業をするのは面倒かもしれません。そうであれば、`src/main.rs`を修正し、最初のキャリブレーションで得た値をそのまま使い続けてもかまいません。

<!-- Now where we got the sensor calibration out of the way let's look into
actually building this application! -->

ではキャリブレーションができたところで、実際にアプリケーションを作りはじめましょう！
