<!--# Setting up a development environment-->

# 開発環境のセットアップ

<!--Dealing with microcontrollers involves several tools as we'll be dealing with an architecture
different from your computer's and we'll have to run and debug programs on a "remote" device.-->

マイクロコントローラを扱うためには、いくつかのツールを導入せねばなりません。
なぜなら、あなたが今使っているそのコンピュータとは異なったアーキテクチャを相手にすることになるからです。
また、プログラムの実行およびデバッグを、「リモート」のデバイス上でおこなう必要もあります。

<!--## Documentation-->

## ドキュメント

<!-- Tooling is not everything though. Without documentation, it is pretty much impossible to work with
microcontrollers.-->

もっとも、ツールだけでは不十分です。
ドキュメントなくして、マイクロコントローラを触ることは到底できないなのです。

<!--We'll be referring to all these documents throughout this book:-->

この本では、全体を通して以下のドキュメントを参照します。

- [LSM303AGR]

[LSM303AGR]: https://www.st.com/resource/en/datasheet/lsm303agr.pdf

<!-- ## Tools -->

## ツール

<!-- We'll use all the tools listed below. Where a minimum version is not specified, any recent version
should work but we have listed the version we have tested. -->

以下に挙げるツールすべてを使います。
最小バージョンを記載していないものに関しては、最近リリースされたバージョンであれば正常に動くはずです。
ですがここでは、動作確認をしたバージョンを併せて記載しました。

<!-- - Rust 1.53.0 or a newer toolchain.-->

- Rust toolchain 1.53.0以上

<!-- - `gdb-multiarch`. Tested version: 10.2. Other versions will most likely work as well though
  If your distribution/platform does not have `gdb-multiarch` available `arm-none-eabi-gdb`
  will do the trick as well. Furthermore, some normal `gdb` binaries are built with multiarch
  capabilities as well, you can find further information about this in the sub chapters. -->

  - `gdb-multiarch` バージョン10.2で動作確認済み。
他のバージョンでも同様に正しく動作すると思います。
お使いのOSディストリビューションやプラットフォームに対応した`gdb-multiarch`がない場合は、`arm-none-eabi-gdb`で代用が可能です。
さらに、通常の`gdb`バイナリでも、マルチアーキテクチャに対応しているものがあります。
このことについては、サブチャプターにも詳しく記載しています。

<!-- - [`cargo-binutils`]. Version 0.3.3 or newer.-->

- [`cargo-binutils`]  バージョン0.3.3以上。

[`cargo-binutils`]: https://github.com/rust-embedded/cargo-binutils

<!-- - [`cargo-embed`]. Version 0.11.0 or newer. -->

- [`cargo-embed`] バージョン0.11.0以上。

[`cargo-embed`]: https://github.com/probe-rs/cargo-embed

<!-- - `minicom` on Linux and macOS. Tested version: 2.7.1. Other versions will most likely work as well though -->

- LinuxとmacOSの場合：`minicom` バージョン2.7.1.で動作確認済みですが、他のバージョンでも同様に正しく動作すると思います。

<!-- - `PuTTY` on Windows. -->

- Windowsの場合：`PuTTY`

<!-- Next, follow OS-agnostic installation instructions for a few of the tools: -->

続いて、OSに共通である以下の手順に従って、いくつかのツールを導入してください。

### `rustc` & Cargo

<!-- Install rustup by following the instructions at [https://rustup.rs](https://rustup.rs). -->

[https://rustup.rs](https://rustup.rs)に従って、rustupをインストールしてください。

<!-- If you already have rustup installed double check that you are on the stable
channel and your stable toolchain is up-to-date. `rustc -V` should return a date
newer than the one shown below:-->

もしすでにrustupがインストール済みの場合でも、stableであることと、ツールチェーンが最新版であることを、念のため確認してください。
`rustc -V`で得られる実行結果の日付が、以下に示すもの以降になるようにしてください。

``` console
$ rustc -V
rustc 1.53.0 (53cb7b09b 2021-06-17)
```

### `cargo-binutils`

``` console
$ rustup component add llvm-tools-preview

$ cargo install cargo-binutils --vers 0.3.3

$ cargo size --version
cargo-size 0.3.3
```

### `cargo-embed`

```console
$ cargo install cargo-embed --vers 0.11.0

$ cargo embed --version
cargo-embed 0.11.0
git commit: crates.io
```

<!-- ### This repository -->

### この本のリポジトリ

<!-- Since this book also contains some small Rust code bases used in various chapters
you will also have to download its source code. You can do this in one of the following ways:-->

この本では、ちょっとしたRustのコードベースがいろいろなチャプターで使われています。
このため、以下のいずれかの手順に従い、そのソースコードをダウンロードする必要があります。

<!-- * Visit the [repository](https://github.com/rust-embedded/discovery/), click the green "Code" button and then the
   "Download Zip" one -->

* [この本のリポジトリ](https://github.com/rust-embedded/discovery/)にアクセスし、緑色の「Code」ボタン、その後「Download Zip」ボタンを続けてクリックします。
> 訳注：この本の日本語版リポジトリは[こちら](https://github.com/tomoyuki-nakabayashi/discovery)です

<!-- * Clone it using git (if you know git you presumably already have it installed) from the same repository as linked in
   the zip approach-->

* 上記手順と同じリンク先のリポジトリから、gitでcloneします。（もしあなたがgitをご存じであれば、もうすでにインストール済みであることでしょう。）

<!-- ### OS specific instructions -->

### OSごとの手順

<!-- Now follow the instructions specific to the OS you are using: -->

続いて、お使いのOSに対応した手順に従ってください。

- [Linux](linux.md)
- [Windows](windows.md)
- [macOS](macos.md)
