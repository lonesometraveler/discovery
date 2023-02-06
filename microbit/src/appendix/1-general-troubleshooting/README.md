<!--# General troubleshooting-->

# トラブルシューティング

<!--## `cargo-embed` problems-->

## `cargo-embed`の問題


<!--Most `cargo-embed` problems are either related to not having installed the `udev`
rules properly (on Linux) or having selected the wrong chip configuration in `Embed.toml` so
make sure you got both of those right.-->

`cargo-embed`に関した問題のほとんどは、`Embed.toml`で間違ったチップを選択しているか、(Linuxの場合)`udev`ルールを適切にインストールしていないかが原因です。ですから、その両方が正しく設定されていることを確認してください。

<!--If the above does not work out for you, you can open an issue in the [`discovery` issue tracker].
Alternatively you can also visit the [Rust Embedded matrix channel] or the [probe-rs matrix channel]
and ask for help there.-->

それでもまだうまくいかないときは、[`discovery` issue tracker]でイシューを立ててください。また[Rust Embedded matrix channel]や[probe-rs matrix channel]を訪れて、そこで質問していただいてもよいです。

[`discovery` issue tracker]: https://github.com/rust-embedded/discovery/issues
[Rust Embedded matrix channel]: https://matrix.to/#/#rust-embedded:matrix.org
[probe-rs matrix channel]: https://matrix.to/#/#probe-rs:matrix.org

<!--## Cargo problems-->

## Cargoの問題

### "can't find crate for `core`"

<!--#### Symptoms-->

#### 症状

```
   Compiling volatile-register v0.1.2
   Compiling rlibc v1.0.0
   Compiling r0 v0.1.0
error[E0463]: can't find crate for `core`

error: aborting due to previous error

error[E0463]: can't find crate for `core`

error: aborting due to previous error

error[E0463]: can't find crate for `core`

error: aborting due to previous error

Build failed, waiting for other jobs to finish...
Build failed, waiting for other jobs to finish...
error: Could not compile `r0`.

To learn more, run the command again with --verbose.
```

<!--#### Cause-->

#### 原因

<!--You forgot to install the proper target for your microcontroller (`thumbv7em-none-eabihf` for v2
and `thumbv6m-none-eabi` for v1).-->

お使いのマイクロコントローラ向けのターゲットがインストールされていません。（v2には`thumbv7em-none-eabihf`、v1には`thumbv6m-none-eabi`）

<!--#### Fix-->


#### 解決策

<!--Install the proper target.-->

適切なターゲットをインストールします。

``` console
# micro:bit v2
$ rustup target add thumbv7em-none-eabihf

# micro:bit v1
$ rustup target add thumbv6m-none-eabi
```
