<!-- # Debug it -->
# デバッグする
<!--
## How does this even work?
Before we debug our little program let's take a moment to quickly understand what is actually
happening here. In the previous chapter we already discussed the purpose of the second chip
on the board as well as how it talks to our computer, but how can we actually use it?
-->

## どのような仕組みなのか？
小さなプログラムをデバッグする前に、少し寄り道をして何が起こるのか簡単に理解しましょう。
前章でボード上の2つ目のチップの役割とどうやって私たちのコンピュータとやり取りするか、を説明しましたが果たしてどうやって使うのでしょう？

<!--
The little option `default.gb.enabled = true` in `Embed.toml` made `cargo-embed` open a so-called "GDB stub" after flashing,
this is a server that our GDB can connect to and send commands like "set a breakpoint at address X" to. The server can then decide
on its own how to handle this command. In the case of the `cargo-embed` GDB stub it will forward the
command to the debugging probe on the board via USB which then does the job of actually talking to the
MCU for us.
-->

`Embed.toml`に`default.gb.enabled = true`のオプションを追加すると、フラッシュへの書き込み後、`cargo-embed`は「GDBスタブ」と呼ばれるものを立ち上げます。
GDBスタブはGDBが接続できるサーバーで、「ブレイクポイントをX番地に設定する」といったコマンドをサーバーに送信します。
その後、サーバーはこのコマンドをどう扱うか決めます。
`cargo-embed`の場合、GDBスタブはコマンドをUSB経由でボード上のデバッグプローブに転送し、デバッグプローブが実際にMCUとやり取りします。

<!-- ## Let's debug! -->

## デバッグしてみよう！

<!--
Since `cargo-embed` is blocking our current shell we can simply open a new one and cd back into our project
directory. Once we are there we first have to open the binary in gdb like this:
-->

`cargo-embed`が今使っているシェルをブロックしているので、新しいシェルを立ち上げてプロジェクトディレクトリに移動し直します。
まず最初に、プロジェクトディレクトリに居る状態で、次のようにgdbでバイナリを読み込まなければなりません。

```shell
# For micro:bit v2
$ gdb target/thumbv7em-none-eabihf/debug/led-roulette

# For micro:bit v1
$ gdb target/thumbv6m-none-eabi/debug/led-roulette
```

<!--
> **NOTE** Depending on which GDB you installed you will have to use a different command to launch it,
> check out [chapter 3] if you forgot which one it was.

[chapter 3]: ../03-setup/index.md#tools
-->

> **注意** どのGDBをインストールしたかによって、GDB起動のコマンドが違います。
> どのGDBをインストールしたか忘れた場合は、[第3章]を確認してください。

[第3章]: ../03-setup/index.md#tools

<!--
> **NOTE**: If `cargo-embed` prints a lot of warnings here don't worry about it. As of now it does not fully
> implement the GDB protocol and thus might not recognize all the commands your GDB is sending to it,
> as long as it does not crash, you are fine.
-->

> **注意** もし`cargo-embed`がたくさん警告を出力しても気にしないでください。
> `cargo-embed`はGDBプロトコルを全て実装しているわけではないので、GDBから送信したコマンドが認識できないことがあります。
> `cargo-embed`が異常終了しない限り、問題ありません。

<!--
Next we will have to connect to the GDB stub. It runs on `localhost:1337` per default so in order to
connect to it run the following:
-->

次にGDBスタブに接続しなければなりません。
GDBスタブはデフォルトでは`localhost:1337`で動いており、これに接続するには次のコマンドを実行します。

```shell
(gdb) target remote :1337
Remote debugging using :1337
0x00000116 in nrf52833_pac::{{impl}}::fmt (self=0xd472e165, f=0x3c195ff7) at /home/nix/.cargo/registry/src/github.com-1ecc6299db9ec823/nrf52833-pac-0.9.0/src/lib.rs:157
157     #[derive(Copy, Clone, Debug)]
```

<!--
Next what we want to do is get to the main function of our program.
We will do this by first setting a breakpoint there and the continuing
program execution until we hit the breakpoint:
-->

続いて、プログラムのmain関数に行きたいです。
そのためにまずブレイクポイントをmain関数に設定して、ブレイクポイントに到達するまでプログラムの実行を続けます。

```
(gdb) break main
Breakpoint 1 at 0x104: file src/05-led-roulette/src/main.rs, line 9.
Note: automatically using hardware breakpoints for read-only addresses.
(gdb) continue
Continuing.

Breakpoint 1, led_roulette::__cortex_m_rt_main_trampoline () at src/05-led-roulette/src/main.rs:9
9       #[entry]
```

<!--
Breakpoints can be used to stop the normal flow of a program. The `continue` command will let the
program run freely *until* it reaches a breakpoint. In this case, until it reaches the `main`
function because there's a breakpoint there.
-->

ブレイクポイントはプログラムのフローを止めるために使えます。
`continue`コマンドはブレイクポイントに到達する*まで*、プログラムを自由に実行します。
この場合、`main`関数にブレイクポイントがあるので、そこに到達するまでプログラムを実行します。

<!--
Note that GDB output says "Breakpoint 1". Remember that our processor can only use a limited amount of these
breakpoints, so it's a good idea to pay attention to these messages. If you happen to run out of breakpoints,
you can list all the current ones with `info break` and delete desired ones with `delete <breakpoint-num>`.
-->

GDBが「Breakpoint 1」と出力していることに注意してください。
今使っているプロセッサでは限られた数のブレイクポイントしか使えないことを覚えておいてください。
先程のようなメッセージに注目を払うのは良いアイデアです。
もしブレイクポイントを使い果たしてしまったら、`info break`で現在設定しているブレイクポイントの一覧が見れます。
そして`delete <ブレイクポイント番号>`でお望みのブレイクポイントを削除します。

<!--
For a nicer debugging experience, we'll be using GDB's Text User Interface (TUI). To enter into that
mode, on the GDB shell enter the following command:
-->

より良いデバッグを体験するために、GDBのテキストユーザーインターフェース（TUI）を使ってみましょう。
TUIモードに切り替えるために、GDBシェルに次のコマンドを入力します。

```
(gdb) layout src
```

<!--
> **NOTE** Apologies Windows users. The GDB shipped with the GNU ARM Embedded Toolchain doesn't
> support this TUI mode `:-(`.
-->

> **注意** Windowsユーザーのみなさんごめんなさい。GNU ARM Embeddedツールチェインで配布されているGDBではTUIモードがサポートされていません `:-(`。

<!--
![GDB session](../assets/gdb-layout-src.png "GDB TUI")
-->

![GDBセッション](../assets/gdb-layout-src.png "GDB TUI")

<!--
GDB's break command does not only work for function names, it can also break at certain line numbers.
If we wanted to break in line 13 we can simply do:
-->

GDBのブレイクコマンドは関数名だけでなく、特定の行番号でも効果を発揮します。
もし13行目でブレイクしたければ、単に次のようにします。

```
(gdb) break 13
Breakpoint 2 at 0x110: file src/05-led-roulette/src/main.rs, line 13.
(gdb) continue
Continuing.

Breakpoint 2, led_roulette::__cortex_m_rt_main () at src/05-led-roulette/src/main.rs:13
(gdb)
```
<!--
At any point you can leave the TUI mode using the following command:
-->

次のコマンドでTUIモードをいつでも終了できます。

```
(gdb) tui disable
```

<!--
We are now "on" the `_y = x` statement; that statement hasn't been executed yet. This means that `x`
is initialized but `_y` is not. Let's inspect those stack/local variables using the `print` command:
-->

現在`_y = x`の文の「上」にいます。
この文はまだ実行されていません。
そのため、`x`は初期化されていますが、`_y`は初期化されていません。
`print`コマンドを使って、これらのスタック/ローカル変数を調べてみましょう。

```
(gdb) print x
$1 = 42
(gdb) print &x
$2 = (*mut i32) 0x20003fe8
(gdb)
```

<!--
As expected, `x` contains the value `42`. The command `print &x` prints the address of the variable `x`.
The interesting bit here is that GDB output shows the type of the reference: `i32*`, a pointer to an `i32` value.
-->

期待通り、`x`は値`42`を格納しています。
`print &x`コマンドは変数`x`のアドレスを表示します。
ここでちょっとおもしろいところは、GDBが`i32*`という型を表示している点です。
これは`i32`のポインタ型です。

<!--
If we want to continue the program execution line by line we can do that using the `next` command
so let's proceed to the `loop {}` statement:
-->

プログラムの実行を1行ずつ続けたい場合は、`next`コマンドを使います。
それでは、`loop {}`文まで進んでみましょう。

```
(gdb) next
16          loop {}
```

<!--
And `_y` should now be initialized.
-->

すると、`_y`が初期化されています。

```
(gdb) print _y
$5 = 42
```

<!--
Instead of printing the local variables one by one, you can also use the `info locals` command:
-->

1つずつローカル変数を表示する代わりに、`info locals`コマンドを使うことができます。

```
(gdb) info locals
x = 42
_y = 42
(gdb)
```

<!--
If we use `next` again on top of the `loop {}` statement, we'll get stuck because the program will
never pass that statement. Instead, we'll switch to the disassemble view with the `layout asm`
command and advance one instruction at a time using `stepi`. You can always switch back into Rust
source code view later by issuing the `layout src` command again.
-->

`loop {}`文で`next`を再び実行すると、そこから操作できなくなります。
これは`loop {}`文を抜けることがないためです。
代わりに、`layout asm`コマンドでディスアンセンブル画面に切り替えて、`stepi`コマンドを使って1命令ずつ実行を進めます。
`layout src`コマンドで、Rustソースコード画面にいつでも戻ってくることができます。

<!--
> **NOTE**: If you used the `next` or `continue` command by mistake and GDB got stuck, you can get unstuck by hitting `Ctrl+C`.
-->

> **注意**: 間違って`next`や`continue`コマンドを使ってしまいGDBが操作できなくなった場合は、`Ctrl+C`で再びGDBを操作できます。


```
(gdb) layout asm
```

<!-- ![GDB session](../assets/gdb-layout-asm.png "GDB disassemble") -->

![GDB session](../assets/gdb-layout-asm.png "GDBディスアセンブル")

<!--
If you are not using the TUI mode, you can use the `disassemble /m` command to disassemble the
program around the line you are currently at.
-->

TUIモードを使わない場合、`disassemble /m`コマンドで現在実行している行付近のプログラムをディスアセンブルできます。

```
(gdb) disassemble /m
Dump of assembler code for function _ZN12led_roulette18__cortex_m_rt_main17h3e25e3afbec4e196E:
10      fn main() -> ! {
   0x0000010a <+0>:     sub     sp, #8
   0x0000010c <+2>:     movs    r0, #42 ; 0x2a

11          let _y;
12          let x = 42;
   0x0000010e <+4>:     str     r0, [sp, #0]

13          _y = x;
   0x00000110 <+6>:     str     r0, [sp, #4]

14
15          // infinite loop; just so we don't leave this stack frame
16          loop {}
=> 0x00000112 <+8>:     b.n     0x114 <_ZN12led_roulette18__cortex_m_rt_main17h3e25e3afbec4e196E+10>
   0x00000114 <+10>:    b.n     0x114 <_ZN12led_roulette18__cortex_m_rt_main17h3e25e3afbec4e196E+10>

End of assembler dump.
```

<!--
See the fat arrow `=>` on the left side? It shows the instruction the processor will execute next.
-->

左側に太矢印の`=>`があるところを見てください。
これはプロセッサが次に実行する命令です。

<!--
If not inside the TUI mode on each `stepi` command GDB will print the statement and the line number
of the instruction the processor will execute next.
-->

TUIモードじゃない場合、`stepi`コマンドを実行するたびに、GDBはプロセッサが次に実行する命令の文と行番号を表示します。

```
(gdb) stepi
16          loop {}
(gdb) stepi
16          loop {}
```

<!--
One last trick before we move to something more interesting. Enter the following commands into GDB:
-->

より楽しい内容に行く前に、最後のトリックを紹介します。
次のコマンドをGDBに入力してください。

```
(gdb) monitor reset
(gdb) c
Continuing.

Breakpoint 1, led_roulette::__cortex_m_rt_main_trampoline () at src/05-led-roulette/src/main.rs:9
9       #[entry]
(gdb)
```

<!--
We are now back at the beginning of `main`!
-->

`main`の最初に戻っています！

<!--
`monitor reset` will reset the microcontroller and stop it right at the program entry point.
The following `continue` command will let the program run freely until it reaches the `main`
function that has a breakpoint on it.
-->

`monitor reset`はマイクロコントローラをリセットし、プログラムのエントリーポイントで停止します。
続く`continue`コマンドでプログラムを`main`に到達するまで実行し、ブレイクポイントのある`main`で止まります。

<!--
This combo is handy when you, by mistake, skipped over a part of the program that you were
interested in inspecting. You can easily roll back the state of your program back to its very
beginning.
-->

このコンボは、プログラムの調査したいところを間違ってスキップしてしまったときに便利です。
簡単にプログラムを最初の状態に戻すことが出来ます。

<!--
> **The fine print**: This `reset` command doesn't clear or touch RAM. That memory will retain its
> values from the previous run. That shouldn't be a problem though, unless your program behavior
> depends on the value of *uninitialized* variables but that's the definition of Undefined Behavior
> (UB).
-->

> **補足**: `reset`コマンドはRAMをクリアしません。
> RAMは`reset`コマンド実行前と同じ値を保持しています。
> プログラムが*初期化されていない*変数の値に依存していない限り、問題にはなりません。
> ただし、そのような状態は未定義動作（Undefined Behavior: UB）と定義付けられています。

<!--
We are done with this debug session. You can end it with the `quit` command.
-->

デバッグセッションを完了しました。
`quit`コマンドでデバッグセッションを終了できます。

```
(gdb) quit
A debugging session is active.

        Inferior 1 [Remote target] will be detached.

Quit anyway? (y or n) y
Detaching from program: $PWD/target/thumbv7em-none-eabihf/debug/led-roulette, Remote target
Ending remote debugging.
[Inferior 1 (Remote target) detached]
```

<!--
> **NOTE** If the default GDB CLI is not to your liking check out [gdb-dashboard]. It uses Python to
> turn the default GDB CLI into a dashboard that shows registers, the source view, the assembly view
> and other things.
-->

> **ノート** デフォルトのGDB CLIがお気に召さない場合は、[gdb-dashboard]をチェックしてみて下さい。
> これはPythonを使って、デフォルトのGDB CLIをレジスタやソースコード表示画面、アセンブリ表示画面などをダッシュボード化してくれます。

[gdb-dashboard]: https://github.com/cyrus-and/gdb-dashboard#gdb-dashboard

<!--
If you want to learn more about what GDB can do, check out the section [How to use GDB](../appendix/2-how-to-use-gdb/).
-->

GDBでできることについてさらに知りたい場合、[GDBの使い方](../appendix/2-how-to-use-gdb/)を参照して下さい。

<!--
What's next? The high level API I promised.
-->

さて、お次は？
お約束した高レベルのAPIです。
