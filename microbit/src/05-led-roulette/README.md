<!-- # LED roulette -->

# LEDルーレット
<!--
Alright, let's start by building the following application:
-->

それでは、次のアプリケーションを作ることから始めましょう。

<p align="center">
<video src="../assets/roulette_fast.mp4" loop autoplay>
</p>

<!--
I'm going to give you a high level API to implement this app but don't worry we'll do low level
stuff later on. The main goal of this chapter is to get familiar with the *flashing* and debugging
process.
-->

このアプリを実装するために高機能なAPIを提供しますが、低レベルなことは後々やっていきますのでご心配なく。
この章の主な目的は*フラッシュへの書き込み*とデバッグに慣れることです。

<!--
The starter code is in the `src` directory of the book repository. Inside that directory there are more
directories named after each chapter of this book. Most of those directories are starter Cargo
projects.
-->

ではこの本のレポジトリの`src`ディレクトリにあるコードから始めましょう。
ディレクトリ内に、各章の名前を冠したディレクトリがあります。
それらのディレクトリのほとんどはCargoのスタータープロジェクトです。

<!--
Now, jump into the `src/05-led-roulette` directory. Check the `src/main.rs` file:
-->

早速、`src/05-led-roulette`ディレクトリの内容に入っていきましょう。
`src/main.rs`ファイルを開いてみてください。

``` rust
{{#include src/main.rs}}
```

<!--
Microcontroller programs are different from standard programs in two aspects: `#![no_std]` and
`#![no_main]`.
-->

マイクロコントローラのプログラムは通常のプログラムと比べて2つの点で異なります。
`#![no_std]`と`#![no_main]`です。

<!--
The `no_std` attribute says that this program won't use the `std` crate, which assumes an underlying
OS; the program will instead use the `core` crate, a subset of `std` that can run on bare metal
systems (i.e., systems without OS abstractions like files and sockets).
-->

`no_std`アトリビュートはこのプログラムがOSの存在を前提とした`std`クレートを使わず、代わりに`std`のサブセットでベアメタルシステム（つまり、ファイルシステムはソケットのようなOSの抽象化がないシステム）でも実行できる`core`クレートを使うことを意味します。

<!--
The `no_main` attribute says that this program won't use the standard `main` interface, which is
tailored for command line applications that receive arguments. Instead of the standard `main` we'll
use the `entry` attribute from the [`cortex-m-rt`] crate to define a custom entry point. In this
program we have named the entry point "main", but any other name could have been used. The entry
point function must have signature `fn() -> !`; this type indicates that the function can't return
-- this means that the program never terminates.
-->

`no_main`アトリビュートはこのプログラムが標準の`main`インターフェースを使わないことを意味しています。
標準の`main`インターフェースは引数を受け取るコマンドラインアプリケーション向けに作られています。
エントリーポイントを定義するために、標準の`main`の代わりに[`cortex-m-rt`]クレートから`entry`アトリビュートを使います。
このプログラムではエントリーポイントの名前を「main」としますが、それ以外の名前も使えます。
エントリーポイントになる関数は`fn() -> !`のシグネチャを持たなければなりません。
このシグネチャは関数が戻れないことを表しており、このプログラムが決して止まらないことを意味します。

[`cortex-m-rt`]: https://crates.io/crates/cortex-m-rt

<!--
If you are a careful observer, you'll also notice there is a `.cargo` directory in the Cargo project
as well. This directory contains a Cargo configuration file (`.cargo/config`) that tweaks the
linking process to tailor the memory layout of the program to the requirements of the target device.
This modified linking process is a requirement of the `cortex-m-rt` crate.
-->

ディレクトリ内をよく見ると、Cargoプロジェクトに`.cargo`ディレクトリがあることに気がつくと思います。
`.cargo`ディレクトリにはCargoの設定ファイル（`.cargo/config`）が含まれています。
このファイルはリンク処理を微調整し、ターゲットデバイス用にプログラムのメモリレイアウトを作成します。
このリンク処理の微調整は、`cortex-m-rt`クレートを使う要件になっています。

<!--
Furthermore, there is also an `Embed.toml` file
-->

さらに、ディレクトリには`Embed.toml`ファイルがあります。

```toml
{{#include Embed.toml}}
```

<!--
This file tells `cargo-embed` that:

* we are working with either a nrf52833 or nrf51822, you will again have to remove the comments from the
  chip you are using, just like you did in chapter 3.
* we want to halt the chip after we flashed it so our program does not instantly jump to the loop
* we want to disable RTT, RTT being a protocol that allows the chip to send text to a debugger.
  You have in fact already seen RTT in action, it was the protocol that sent "Hello World" in chapter 3.
* we want to enable GDB, this will be required for the debugging procedure

Alright, let's start by building this program.
-->

`Embed.toml`は`cargo-embed`に次の情報を与えます。

* nrf52833もしくはnrf51822のいずれかを使っていること。第3章でやったのと同じように、使用するチップのコメントアウトを外してください。
* チップのフラッシュに書き込んだあとプログラムの実行を停止したいこと。これはプログラムがすぐにループに到達しないようにするためです。
* RTTを無効化したいこと。RTTはチップがデバッガにテキストを送るためのプロトコルです。実は第3章で「Hello World」を送信するのに使ったプロトコルがRTTだったのです。
* GDBを有効化したいこと。これはデバッグするのに必要です。

それでは、プログラムをビルドするところから始めましょう。
