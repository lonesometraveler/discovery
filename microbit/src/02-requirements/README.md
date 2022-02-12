<!--　# Hardware/knowledge requirements　-->

# 必要な知識とハードウェア環境

<!--
The primary knowledge requirement to read this book is to know *some* Rust. It's
hard for me to quantify *some* but at least I can tell you that you don't need
to fully grok generics, but you do need to know how to *use* closures. You also
need to be familiar with the idioms of the [2018 edition], in particular with
the fact that `extern crate` is not necessary in the 2018 edition.
-->

この本を読むために必要な知識は、 *ある程度の*Rustに関する知識です。
*ある程度の*が具体的にどの程度かと聞かれれば難しいところですね。
ジェネリクスを完全に理解している必要はないにしろ、クロージャをどうやって*使う*かは知っているべきといったところでしょうか。
[2018 edition]のイディオムにも慣れていた方がよいでしょう。
特に、2018 editionでは`extern crate`を使わなくてもいいということは理解しておいてほしいです。


[2018 edition]: https://rust-lang-nursery.github.io/edition-guide/

<!-- Also, to follow this material you'll need the following hardware: -->

それと、この本の内容を実践するにあたり、以下のハードウェアが必要です。

<!--
- A [micro:bit v2] board, alternatively a [micro:bit v1.5] board, the book
  will refer to the v1.5 as just v1.
-->

- [micro:bit v2]ボードが一つ。代わりに[micro:bit v1.5]ボードでも大丈夫です。この本の中では、v1.5をv1と表記します。

[micro:bit v2]: https://tech.microbit.org/hardware/
[micro:bit v1.5]: https://tech.microbit.org/hardware/1-5-revision/

<!--(You can purchase this board from several [electronics][0] [suppliers][1])---->

([ここ][0]や[ここ][1]など、いくつかの電子部品販売店から購入可能です。)
> 訳注：日本だと[秋月電子通商][2]や[スイッチサイエンス][3]から購入可能です。

[0]: https://microbit.org/buy/
[1]: https://www.mouser.com/microbit/_/N-aez3t?P=1y8um0l
[2]: https://akizukidenshi.com/catalog/g/gM-15882/
[3]: https://www.switch-science.com/catalog/6600/

<p align="center">
<img title="micro:bit" src="../assets/microbit-v2.jpg">
</p>

<!-- > **NOTE** This is an image of a micro:bit v2, the front of the v1 looks slightly different -->

> **注意** 写真はmicro:bit v2のものです。v1の前面はちょっと違う見た目をしています。

<!-- - One micro-B USB cable, required to make the micro:bit board work.
  Make sure that the cable supports data transfer as some cables only support charging devices.
  -->

- micro-BのUSBケーブルが一本、micro:bitを動かすために必要です。充電しかできないものもあるので、データ転送に対応したケーブルであることを確認してください。

<p align="center">
<img title="micro-B USB cable" src="../assets/usb-cable.jpg">
</p>

<!--
> **NOTE** You may already have a cable like this, as some micro:bit kits ship with such cables.
> Some USB cables used to charge mobile devices may also work, if they are micro-B and have the
> capability to transmit data.
-->

> **注意** ケーブルはmicro:bitのキットに同梱されている場合もあります。
> モバイル機器の充電に使っているようなmicro-Bのケーブルでも、実はデータ転送に対応していて使うことができるという場合もあります。

<!--
> **FAQ**: Wait, why do I need this specific hardware?
-->

> **FAQ**: ちょっと待ってください。なぜこの特定のハードウェアが必要なのでしょうか？

<!-- It makes my life and yours much easier. -->

その方が私にとっても、あなたにとっても非常に楽だからです。

<!--
The material is much, much more approachable if we don't have to worry about hardware differences.
Trust me on this one.
-->

ハードウェアの違いを気にしなくていいのであれば、この本はとても、とても取り組みやすくなります。
間違いなく、です。

<!--
> **FAQ**: Can I follow this material with a different development board?
-->

> **FAQ**: 別の開発ボードを使ってこの本の内容に取り組んでも問題ないでしょうか？

<!--Maybe? It depends mainly on two things: your previous experience with microcontrollers and/or
whether a high level crate already exists, like the [`nrf52-hal`], for your development board
somewhere. You can look through the [Awesome Embedded Rust HAL list] for your microcontroller,
if you intend to use a different one.-->

おそらく問題ないはず？です。
あなたがマイクロコントローラをこれまでにどれだけ触ったことがあるか、あるいは（および）、あなたが使おうとしている開発ボードに、[`nrf52-hal`]のような高レベルなクレートがすでにあるか次第だと言えるかもしれません。
もし違うマイクロコントローラを使おうとしているのであれば、 [Awesome Embedded Rust HAL list]でそのようなクレートを探してみるのもいいでしょう。

[`nrf52-hal`]: https://docs.rs/nrf52-hal
[Awesome Embedded Rust HAL list]: https://github.com/rust-embedded/awesome-embedded-rust#hal-implementation-crates

<!--With a different development board, this text would lose most if not all its beginner friendliness
and "easy to follow"-ness, IMO.-->

違う開発ボードを使う場合、この本の持つ、初心者にとっての取り組みやすさが損なわれてしまう、と私は思います

<!--If you have a different development board and you don't consider yourself a total beginner, you are
better off starting with the [quickstart] project template.-->

もし違う開発ボードを使おうとしていて、かつ全くの初心者ではないという自信があるのであれば、[quickstart]プロジェクトのテンプレートに従って始めるほうがいいでしょう。

[quickstart]: https://rust-embedded.github.io/cortex-m-quickstart/cortex_m_quickstart/
