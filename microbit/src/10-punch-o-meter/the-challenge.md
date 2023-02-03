<!-- # The challenge -->

# 課題

<!-- To keep things simple, we'll measure the acceleration only in the X axis while the board remains
horizontal. That way we won't have to deal with subtracting that *fictitious* `1g` we observed
before which would be hard because that `1g` could have X Y Z components depending on how the board
is oriented. -->

話を簡単にするために、ここではボードを水平に保った状態でX軸の加速度だけを測ることにします。そうすることで、前のページで確認した*架空の* `1g`を引く必要がなくなります。架空の`1g`はボードの向きによってX Y Zのどれにでも現れるので、補正するのはむずかしいです。

<!-- Here's what the punch-o-meter must do: -->

パンチングマシンがやるべきことは、次の通りです。

<!-- 
- By default, the app is not "observing" the acceleration of the board.
- When a significant X acceleration is detected (i.e. the acceleration goes above some threshold),
  the app should start a new measurement.
- During that measurement interval, the app should keep track of the maximum acceleration observed
- After the measurement interval ends, the app must report the maximum acceleration observed. You
  can report the value using the `rprintln!` macro.
-->

- デフォルトでは、アプリケーションはボードの加速度を「観測」していません。
- X軸に大きな（しきい値を超える）加速度が感知されたとき、アプリケーションは計測をはじめます。
- 計測中、アプリケーションは最大加速度を記録、更新し続けます。
- 計測期間が終わると、アプリケーションは最大加速度を報告します。報告は`rprintln!`マクロを使って行います。

<!-- Give it a try and let me know how hard you can punch `;-)`. -->

では実装して、あなたのパンチがどのくらい強いか教えてください。`;-)`

<!-- 
> **NOTE** There are two additional APIs that should be useful for this task we haven't discussed yet.
> First the [`set_accel_scale`] one which you need to measure high g values.
> Secondly the [`Countdown`] trait from `embedded_hal`. If you decide to use this to keep your measurement
> intervals you will have to pattern match on the [`nb::Result`] type instead of using the `block!` macro
  we have seen in previous chapters.
 -->
 
 > **注** まだ紹介していないAPIに、便利なものがふたつあります。
> ひとつ目は[`set_accel_scale`]。大きな`g`を測定するときに必要になります。
> ふたつ目は`embedded_hal`の[`Countdown`]トレイトです。これを計測時間のカウントダウンに使用する場合は、これまでの章でしたように`block!`マクロを使うのではなく、[`nb::Result`]型をパターンマッチで処理する必要があります。

[`set_accel_scale`]: https://docs.rs/lsm303agr/0.2.2/lsm303agr/struct.Lsm303agr.html#method.set_accel_scale
[`Countdown`]: https://docs.rs/embedded-hal/0.2.6/embedded_hal/timer/trait.CountDown.html
[`nb::Result`]: https://docs.rs/nb/1.0.0/nb/type.Result.html
