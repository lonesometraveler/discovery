<!-- # Punch-o-meter -->

# パンチングマシン

<!-- In this section we'll be playing with the accelerometer that's in the board. -->

この章ではボードに載っている加速度計を使って遊んでみます。

<!-- What are we building this time? A punch-o-meter! We'll be measuring the power of your jabs. Well,
actually the maximum acceleration that you can reach because acceleration is what accelerometers
measure. Strength and acceleration are proportional though so it's a good approximation. -->

なにを作ると思いますか？　パンチングマシンです！　あなたのジャブの強さを測定します。とは言っても、実際に測定するのは最大加速度です。加速度計が計測するのは加速度ですからね。ですが強さと加速度は比例するので、目安としてはいいでしょう。

<!-- As we already know from previous chapters the accelerometer is built inside the LSM303AGR package.
And just like the magnetometer, it is accessible using the I2C bus. It also has the same coordinate
system as the magnetometer. -->

これまでの章から、加速度計がLSM303AGRパッケージに内蔵されていることはご存知のことと思います。磁力計と同じように、加速度計にもI2Cバスでアクセスします。また加速度計も、磁力計と同じ座標系を持っています。

