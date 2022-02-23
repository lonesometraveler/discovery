<!-- # Nordic nRF52833 (the "nRF52", micro:bit v2) -->

# Nordic nRF52833 (「nRF52」, micro:bit v2)

<!--
Our MCU has 73 tiny metal **pins** sitting right underneath it (it's a so called [aQFN73] chip).
These pins are connected to **traces**, the little "roads" that act as the wires connecting components
together on the board. The MCU can dynamically alter the electrical properties
of the pins. This works similar to a light switch altering how electrical
current flows through a circuit. By enabling or disabling electrical current to
flow through a specific pin, an LED attached to that pin (via the traces) can
be turned on and off.
-->

このMCUの下には73個の小さな金属**ピン**があります（そのため、[aQFN73]と呼ばれます）。
これらのピンは**トレース**と接続しています。トレースとはボード上の部品をつなぐ配線として機能する小さな「道」です。
MCUはピンの電気的特性を動的に変えることができます。
これは照明のスイッチのようなもので、回路上の電流の流れを変えます。
特定のピンに電流を流したり、電流を止めたりすることで、トレース経由でピンと接続しているLEDを点灯したり、消灯したりできます。

<!--
Each manufacturer uses a different part numbering scheme, but many will allow
you to determine information about a component simply by looking at the part
number. Looking at our MCU's part number (`N52833 QIAAA0 2024AL`, you probably cannot
see it with your bare eye, but it is on the chip), the `n` at the
front hints to us that this is a part manufactured by [Nordic Semiconductor].
Looking up the part number on their website we quickly find the [product page].
There we learn that our chip's main marketing point is that it is a
"Bluetooth Low Energy and 2.4 GHz SoC" (SoC being short for "System on a Chip"),
which explains the RF in the product name since RF is short for radio frequency.
If we search through the documentation of the chip linked on the [product page]
for a bit we find the [product specification] which contains chapter 10 "Ordering Information"
dedicated to explaining the weird chip naming. Here we learn that:

[aQFN73]: https://en.wikipedia.org/wiki/Flat_no-leads_package
[Nordic Semiconductor]: https://www.nordicsemi.com/
[product page]: https://www.nordicsemi.com/products/nrf52833
[product specification]: https://infocenter.nordicsemi.com/pdf/nRF52833_PS_v1.3.pdf

- The `N52` is the MCU's series, indicating that there are other `nRF52` MCUs
- The `833` is the part code
- The `QI` is the package code, short for `aQFN73`
- The `AA` is the variant code, indicating how much RAM and flash memory the MCU has,
  in our case 512 kilobyte flash and 128 kilobyte RAM
- The `A0` is the build code, indicating the hardware version (`A`) as well as the product configuration (`0`)
- The `2024AL` is a tracking code, hence it might differ on your chip
-->

各製造メーカーごとに異なる部品番号の付け方をしていますが、多くの場合、部品番号を見るだけで、その部品についての情報を得られます。
今回使用するMCUの部品番号（`N52833 QIAAA0 2024AL`）を見てみましょう（肉眼では見えないかもしれませんが、チップに記載されています）。
最初の`n`は部品が[Nordic Semiconductor]によって製造された、というヒントになっています。
ウェブサイトで部品番号を調べると、すぐにNordic Semiconductorの[製品ページ]が見つかります。
ここで、このチップの売り込みポイントが「Bluetooth Low Energy and 2.4GHz SoC」であることがわかります（SoCは「System on a Chip」の略です）。
製品名に含まれる「RF」はradio frequency（無線周波数）の省略です。
もし、[製品ページ]からリンクされているチップのドキュメントを少し検索してみると、[製品仕様]が見つかります。
その10章「Ordering Information（注文情報）」に奇妙なチップの命名規則について説明があり、次のことがわかります。

[aQFN73]: https://en.wikipedia.org/wiki/Flat_no-leads_package
[Nordic Semiconductor]: https://www.nordicsemi.com/
[製品ページ]: https://www.nordicsemi.com/products/nrf52833
[製品仕様]: https://infocenter.nordicsemi.com/pdf/nRF52833_PS_v1.3.pdf

- `N52`はMCUシリーズで、他の`nRF52` MCUがあることを意味している
- `833`は部品コード
- `QI`はパッケージコードで`aQFN73`の略
- `AA`は種別コードで、MCUがもつRAMとフラッシュの容量を示す。今回の場合、512キロバイトのフラッシュと128キロバイトのRAM
- `A0`はビルドコードで、`A`はハードウェアバージョン、`0`は製品コンフィグレーションを意味する
- `2024AL`はトラッキングコードなので、あなたのチップとは異なるでしょう

<!--
The product specification does of course contain a lot more useful information about
the chip, for example that it is based on an ARM® Cortex™-M4 32-bit processor.
-->

もちろん製品仕様には、このチップに関して役立つ情報がたくさん載っています。
例えば、このチップはARM® Cortex™-M4 32ビットプロセッサをベースとしていること、などです。

## Arm? Cortex-M4?

<!--
If our chip is manufactured by Nordic, then who is Arm? And if our chip is the
nRF52833, what is the Cortex-M4?
-->

使用するチップがNordicによって製造されているとすると、Armとは誰なのでしょう？
チップがnRF52833とすると、Cortex-M4とは何なのでしょうか？

<!--
You might be surprised to hear that while "Arm-based" chips are quite
popular, the company behind the "Arm" trademark ([Arm Holdings][]) doesn't
actually manufacture chips for purchase. Instead, their primary business
model is to just *design* parts of chips. They will then license those designs to
manufacturers, who will in turn implement the designs (perhaps with some of
their own tweaks) in the form of physical hardware that can then be sold.
Arm's strategy here is different from companies like Intel, which both
designs *and* manufactures their chips.
-->

「Armベース」のチップは非常に広く使われているので、「Arm」のトレードマークを持つ会社（[Armホールディングス]）が実際のチップを製造していない、と聞くと驚くかもしれません。
Armの主要なビジネスモデルはチップの一部を*設計*することなのです。
彼らは、それらの設計を製造メーカーにライセンスします。
製造メーカーは（おそらく独自の変更を加えて）そのデザインを物理的なハードウェアとして実装し、販売できるようにします。
チップの設計*と*製造とを両方行うインテルのような会社と、Armの戦略とは違っています。

<!--
Arm licenses a bunch of different designs. Their "Cortex-M" family of designs
are mainly used as the core in microcontrollers. For example, the Cortex-M4
(the core our chip is based on) is designed for low cost and low power usage.
The Cortex-M7 is higher cost, but with more features and performance.
-->

Armはいくつかの異なる設計をライセンスしています。
「Cortex-M」はマクロコントローラのコアとしてよく使われる設計です。
例えば、Cortex-M4（今回使用するチップのベースとなっているコア）は低コストかつ低電力な用途向けに設計されています。
Cortex-M7はより高コストですが、より多くの機能と高い性能を有しています。

<!--
Luckily, you don't need to know too much about different types of processors
or Cortex designs for the sake of this book. However, you are hopefully now a
bit more knowledgeable about the terminology of your device. While you are
working specifically with an nRF52833, you might find yourself reading
documentation and using tools for Cortex-M-based chips, as the nRF52833 is
based on a Cortex-M design.

[Arm Holdings]: https://www.arm.com/
-->

幸いなことに、この本を読むために様々なプロセッサやCortexの設計について詳しく知る必要はありません。
しかし、使用するデバイスの専門用語について少し知識が身についたかと思います。
nRF52833はCortex-Mをベースとした設計なので、nRF52833を使って作業を進めると、Cortex-Mベースのチップのドキュメントを読んだり、ツールを使ったりすることがわかるでしょう。

[Armホールディングス]: https://www.arm.com/
