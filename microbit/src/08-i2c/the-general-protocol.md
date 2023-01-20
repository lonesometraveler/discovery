<!-- # General protocol -->

# プロトコルの概要

<!-- The I2C protocol is more elaborate than the serial communication protocol because it has to support
communication between several devices. Let's see how it works using examples: -->

複数デバイス間の通信をサポートするため、I2Cはシリアル通信よりも複雑なプロトコルとなっています。実際にどう動くのか、例を使って見てみましょう。

## Master -> Slave

## マスター　->　スレーブ

<!-- If the master wants to send data to the slave: -->

マスターがスレーブにデータを送りたいとき、次のようになります。

<p align="center">
  <img class="white_bg" height=180 title="I2C bus" src="https://upload.wikimedia.org/wikipedia/commons/3/3e/I2C.svg">
</p>

<!--
1. Master: Broadcast START
2. M: Broadcast slave address (7 bits) + the R/W (8th) bit set to WRITE
3. Slave: Responds ACK (ACKnowledgement)
4. M: Send one byte
5. S: Responds ACK
6. Repeat steps 4 and 5 zero or more times
7. M: Broadcast STOP OR (broadcast RESTART and go back to (2))
-->

1. Ｍ（マスター）：STARTをブロードキャストする
2. Ｍ：スレーブアドレス（上位７ビット）と、WRITEに設定したR/Wビット（第８ビット）をブロードキャストする
3. Ｓ（スレーブ）：ACK（ACKnowledgement）を返信する
4. Ｍ：１バイト送信する
5. Ｓ：ACKを返信する
6. 必要なだけ４と５を繰り返す
7. Ｍ：STOPをブロードキャストする（または、RESTARTをブロードキャストして２へ戻る）

<!-- > **NOTE** The slave address could have been 10 bits instead of 7 bits long. Nothing else would have
> changed. -->


> **注** スレーブアドレスが７ビット長でなく１０ビット長のときもありますが、他はなにも変わりません。

<!-- ## Master <- Slave -->

## マスター <- スレーブ

<!-- If the master wants to read data from the slave: -->

マスターがスレーブからデータを読み取りたいときはこうなります。

<p align="center">
<img class="white_bg" height=180 title="I2C bus" src="https://upload.wikimedia.org/wikipedia/commons/3/3e/I2C.svg">
</p>

<!--
1. M: Broadcast START
2. M: Broadcast slave address (7 bits) + the R/W (8th) bit set to READ
3. S: Responds with ACK
4. S: Send byte
5. M: Responds with ACK
6. Repeat steps 4 and 5 zero or more times
7. M: Broadcast STOP OR (broadcast RESTART and go back to (2))
8. -->

1. Ｍ：STARTをブロードキャストする
2. Ｍ：スレーブアドレス（上位７ビット）と、READに設定したR/Wビット（第８ビット）をブロードキャストする
3. Ｓ：ACK（ACKnowledgement）を返信する
4. Ｓ：１バイト送信する
5. Ｍ：ACKを返答する
6. 必要なだけ４と５を繰り返す
7. Ｍ：STOPをブロードキャストする（または、RESTARTをブロードキャストして２へ戻る）

<!-- > **NOTE** The slave address could have been 10 bits instead of 7 bits long. Nothing else would have
> changed. -->


> **注** スレーブアドレスが７ビット長でなく１０ビット長のときもありますが、他はなにも変わりません。
