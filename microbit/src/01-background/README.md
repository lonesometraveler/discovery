<!-- # Background -->

# 背景

<!-- ## What's a microcontroller? -->

## マイクロコントローラとは？

<!--
A microcontroller is a *system* on a chip. Whereas your computer is made up of several discrete
components: a processor, RAM, storage, an Ethernet port, etc.; a microcontroller has all those types
of components built into a single "chip" or package. This makes it possible to build systems with
fewer parts.
-->

マイクロコントローラは、1チップ上の*システム*です。
あなたのコンピュータがプロセッサ、RAM、ストレージ、イーサーネットポートなど、いくつかの個別の部品で構成されている一方、マイクロコントローラはそれらの構成部品を1つの「チップ」またはパッケージに組み込んでいます。
このことにより、より少ない部品数でシステムを構築できます。

<!-- ## What can you do with a microcontroller? -->

## マイクロコントローラでできることは？

<!--
Lots of things! Microcontrollers are the central part of what are known as "*embedded* systems".
Embedded systems are everywhere, but you don't usually notice them. They control the machines that
wash your clothes, print your documents, and cook your food. Embedded systems keep the buildings
that you live and work in at a comfortable temperature, and control the components that make the
vehicles you travel in stop and go.
-->

多くのことができます！
マイクロコントローラは「*組込み*システム」として知られているものの中心になる部品です。
組込みシステムはどこにでもありますが、通常それらに気づくことはありません。
組込みシステムは、衣服を選択したり、書類を印刷したり、食べ物を調理します。
組込みシステムは、生活し働く建物を快適な温度に保ち、自動車を走らせる部品を制御します。

<!--
Most embedded systems operate without user intervention. Even if they expose a user interface like a
washing machine does; most of their operation is done on their own.
-->

ほとんどの組込みシステムはユーザーの介入なしに動作します。
洗濯機のようにユーザーインタフェースがある場合でされ、ほとんどの動作は組込みシステムだけで完結します。

<!--
Embedded systems are often used to *control* a physical process. To make this possible, they have
one or more devices to tell them about the state of the world ("sensors"), and one or more
devices which allow them to change things ("actuators"). For example, a building climate control
system might have:
-->

組込みシステムは物理的な処理を*制御*するのに、よく使われます。
そのため、組込みシステムは世界の状態を知るためのデバイス（「センサ」）と物を動かすためのデバイス（「アクチュエータ」）を持ちます。
例えば、建物の気候を制御するシステムは次のデバイスを持つでしょう。

<!--
- Sensors which measure temperature and humidity in various locations.
- Actuators which control the speed of fans.
- Actuators which cause heat to be added or removed from the building.
-->

- 複数の場所で温度と湿度を計測するためのセンサ
- ファンの速度を制御するアクチュエータ
- 建物の熱を取り込んだり排出するアクチュエータ

<!-- ## When should I use a microcontroller? -->

## いつマイクロコントローラを使うべきなのでしょうか？

<!--
Many of the embedded systems listed above could be implemented with a computer running Linux (for
example a "Raspberry Pi"). Why use a microcontroller instead? Sounds like it might be harder to
develop a program.
-->

上述したほとんどの組込みシステムはLinuxが動いているコンピュータ（例えば「Raspberry Pi」）で実装することができます。
代わりにマイクロコントローラを使うのはなぜでしょうか？
プログラムの開発がより大変に思えます。

<!--
Some reasons might include:
-->

いくつかの理由があります。

<!--
**Cost.** A microcontroller is much cheaper than a general purpose computer. Not only is the
microcontroller cheaper; it also requires many fewer external electrical components to operate.
This makes Printed Circuit Boards (PCB) smaller and cheaper to design and manufacture.
-->

**コスト**。マイクロコントローラは汎用コンピュータよりずっと安価です。
マイクロコントローラは安価なだけでなく、動作に必要となる外部電気部品がずっと少ないです。
そのため、プリント基板（PCB）を小さく安価に、設計し製造できます。

<!--
**Power consumption.** Most microcontrollers consume a fraction of the power of a full blown
processor. For applications which run on batteries, that makes a huge difference.
-->

**消費電力**。ほとんどのマイクロコントローラは本格的なプロセッサと比べるとほんの少しの電力しか使いません。
バッテリで動作するアプリケーションにとって、これは大きな違いです。

<!--
**Responsiveness.** To accomplish their purpose, some embedded systems must always react within a
limited time interval (e.g. the "anti-lock" breaking system of a car). If the system misses this
type of *deadline*, a catastrophic failure might occur. Such a deadline is called a "hard real time"
requirement. An embedded system which is bound by such a deadline is referred to as a "hard
real-time system". A general purpose computer and OS usually has many software components which
share the computer's processing resources. This makes it harder to guarantee execution of a program
within tight time constraints.
-->

**応答性**。組込みシステムの中には、その目的を果たすため、常に限られた時間間隔で応答しなければならないものもあります（例えば車の「アンチロック」ブレーキシステムです）。
もしシステムがこの*デッドライン*に間に合わないと、悲惨な結末を迎えるでしょう。
このようなデッドラインは「ハードリアルタイム」要求と呼ばれています。
このようなデッドラインの制約がある組込みシステムは「ハードリアルタイムシステム」と呼ばれます。
汎用コンピュータと汎用OSは通常、多くのソフトウェアコンポーネントでコンピュータの処理資源を共有します。
そのため、厳密な時間制約内でのプログラム実行を保証するのが難しいです。

<!--
**Reliability.** In systems with fewer components (both hardware and software), there is less to go
wrong!
-->

**信頼性**。より部品が少ないシステム（ハードウェアとソフトウェアの両方）では、間違いが起こりにくくなります！

<!-- ## When should I *not* use a microcontroller? -->

## マイクロコントローラを使うべきで*ない*時はいつでしょうか？

<!--
Where heavy computations are involved. To keep their power consumption low, microcontrollers have
very limited computational resources available to them. For example, some microcontrollers don't
even have hardware support for floating point operations. On those devices, performing a simple
addition of single precision numbers can take hundreds of CPU cycles.
-->

計算量が膨大な場合です。
消費電力を低く抑えるため、マイクロコントローラは非常に限られた計算資源しか持ちません。
例えば、浮動小数点演算を提供するハードウェアすらないマイクロコントローラもあります。
そのようなデバイスでは単精度浮動小数点数の単純な加算ですら、数百CPUサイクルかかります。

<!-- ## Why use Rust and not C? -->

## CではなくRustを使う理由はなんでしょうか？

<!--
Hopefully, I don't need to convince you here as you are probably familiar with the language
differences between Rust and C. One point I do want to bring up is package management. C lacks an
official, widely accepted package management solution whereas Rust has Cargo. This makes development
*much* easier. And, IMO, easy package management encourages code reuse because libraries can be
easily integrated into an application which is also a good thing as libraries get more "battle
testing".
-->

あなたが既にRustとCとの違いを知っており、ここで説得する必要がないことを願っています。
あえて1つ強調すると、それはパッケージ管理システムです。
RustにはCargoがある一方、C言語は公式の広く普及しているパッケージ管理システムがありません。
パッケージ管理システムがあることは開発を*非常*に楽にします。
私の意見としては、パッケージ管理が簡単であることは、コードの再利用を促進します。
なぜなら、ライブラリがアプリケーションに容易に結合できるからです。
このことは、ライブラリがより「実戦で使われる」ことにも良い影響があります。

<!-- ## Why should I not use Rust? -->

## Rustを使うべきでない理由は何でしょうか？

<!--
Or why should I prefer C over Rust?
-->

もしくは、RustよりCを選ぶ理由はなんでしょうか？

<!--
The C ecosystem is way more mature. Off the shelf solution for several problems already exist. If
you need to control a time sensitive process, you can grab one of the existing commercial Real Time
Operating Systems (RTOS) out there and solve your problem. There are no commercial, production-grade
RTOSes in Rust yet so you would have to either create one yourself or try one of the ones that are
in development. You can find a list of those in the [Awesome Embedded Rust] repository.
-->

C言語のエコシステムはより成熟しています。
多くの問題に対して、既に解決策が存在しています。
時間制約のあるプロセスを制御する必要がある場合、既存の商用リアルタイムOS（RTOS）を選び、問題を解決することができます。
Rustにはまだ、商用で製品レベルのRTOSがないため、自分自身で作るか、開発中のものを試す必要があります。
[Awesome Embedded Rust]リポジトリにはそれらのリストがあります。

[Awesome Embedded Rust]: https://github.com/rust-embedded/awesome-embedded-rust#real-time-operating-system-rtos
