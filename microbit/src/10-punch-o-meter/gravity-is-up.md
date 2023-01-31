<!-- # Gravity is up? -->

# 重力は上を向いている？

<!-- What's the first thing we'll do? -->

最初にすべきことはなんでしょうか？

<!-- Perform a sanity check! -->

センサの基本動作の確認です！

<!-- You should already be able to write a program that continuously prints the accelerometer
data on the RTT console from the [I2C chapter](../08-i2c/index.md). Do you observe something
interesting even when holding the board parallel to the floor with the LED side facing down? -->

すでに[I2Cの章](../08-i2c/index.md)でやりましたから、加速度計の値をRTTコンソールにプリントし続けるのはできるはずです。LEDの側を下に向け、地面に対して水平に持つとき、なにかおもしろい現象を確認できるでしょうか？

<!-- What you should see like this is that both the X and Y values are rather close to 0, while the
Z value is at around 1000. Which is weird because the board is not moving yet its acceleration is
non-zero. What's going on? This must be related to the gravity, right? Because the acceleration of
gravity is `1 g` (aha, `1 g` = 1000 from the accelerometer). But the gravity pulls objects downwards
so the acceleration along the Z axis should be negative not positive -->

XとYの値は両方とも0に近い値で、Zは1000に近い値となっているはずです。ボードが動いていないのに、加速度がゼロではないとは変です。なにが起きているのでしょうか？　重力に関係している、ですよね？　そうです、重力の加速度が`1 g` （`1 g`= 加速度計が報告する1000）だからです。ですが、重力は物体を下に引くものです。そうするとZ軸の加速度は、正ではなく負であるべきではないでしょうか。

<!-- Did the program get the Z axis backwards? Nope, you can test rotating the board to align the gravity
to the X or Y axis but the acceleration measured by the accelerometer is always pointing up. -->

プログラムがZ軸を逆に扱っているのでしょうか？　違います。ボードを回転させて、重力がX軸とY軸に添うように持っても、センサで測定された加速度はつねに上を指しています。

<!-- What happens here is that the accelerometer is measuring the *proper acceleration* of the board not
the acceleration *you* are observing. This proper acceleration is the acceleration of the board as
seen from an observer that's in free fall. An observer that's in free fall is moving toward the
center of the Earth with an acceleration of `1g`; from its point of view the board is actually
moving upwards (away from the center of the Earth) with an acceleration of `1g`. And that's why the
proper acceleration is pointing up. This also means that if the board was in free fall, the
accelerometer would report a proper acceleration of zero. Please, don't try that at home. -->

なにが起きているかと言うと、加速度計は*あなたが*観測する加速度ではなく、ボードの*固有加速度*を測定しているのです。固有加速度とは、自由落下中の観測者から見たボードの加速度です。自由落下中の観察者は地球の中心に向けて`1g`の加速度で動いており、この観察者の視点からすると、ボードは`1g`の加速度で上へ動いている（地球の中心から離れていっている）ように見えます。これが、固有加速度が上を向いている理由です。これはつまり、もしもボードが自由落下していれば、加速度計は固有加速度をゼロと報告することを意味します。家では試さないようにしてください。

<!-- Yes, physics is hard. Let's move on. -->

物理は難しいですね。先に進みましょう。
