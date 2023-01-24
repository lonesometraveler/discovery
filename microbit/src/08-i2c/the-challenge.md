<!-- # The challenge -->

# 課題

<!-- The challenge for this chapter is, to build a small application that
communicates with the outside world via the serial interface introduced
in the last chapter. It should be able to receive the commands "magnetometer"
as well as "accelerometer" and then print the corresponding sensor data
in response. This time no template code will be provided since all you need
is already provided in the [UART](../07-uart/index.md) and this chapter. However, here are a few clues: -->

この章での課題です。前章で紹介したシリアルインターフェースを利用して、実世界と通信する小さなアプリケーションを作ってください。"magnetometer"（磁力計）、"accelerometer"（加速度計）のふたつのコマンドを受け取って、対応するセンサの値をプリントすることとします。実装に必要な情報はすでにこの章と[UART](../07-uart/index.md)の章でお渡ししたので、今回はテンプレートはありません。ですが、ヒントは差し上げましょう。

<!-- 
- You might be interested in `heapless::String` since we are working with strings now
- You will (obviously) have to read the documentation of the magnetometer API, however
  it's more or less equivalent to the accelerometer one
-->

- 文字列を扱うことになるので、`heapless::String`を使うといいかもしれません。
- 磁力計のAPIドキュメンテーションを（もちろん）読まなくてはなりません。ですが、加速度計のそれとほとんど変わりありません。
