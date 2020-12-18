# 目次

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [ラズパイゼロでラジコン戦車プログラムを動かす方法](#ラズパイゼロでラジコン戦車プログラムを動かす方法)
  - [はじめに](#はじめに)
  - [ラズパイゼロの初回設定をするにあたって必要なもの](#ラズパイゼロの初回設定をするにあたって必要なもの)
  - [Raspbianのインストール](#raspbianのインストール)
  - [初期設定ツールでの設定](#初期設定ツールでの設定)
  - [Bluetoothのインストール](#bluetoothのインストール)
  - [動くか確認](#動くか確認)

<!-- /code_chunk_output -->

# ラズパイゼロでラジコン戦車プログラムを動かす方法
この章では、Raspberry PiのOSであるRaspbianをインストールするところから、実際にラジコン戦車プログラムを動かすまでの手順を説明していきます。
ここで書かれている内容は、個人ブログ( http://electronic-handicraft.work )にも書かれています。

もうすでに、ラズパイゼロでラジコン戦車プログラムが動くSDカードのデータがあるのであれば、そのデータをそのままイメージファイルに置き換えて、他のSDカードにコピーすればいいので、この章で説明する手順を行う必要はありません。
そのため、毎年、実験アシスタントや体験に来てくれた受講生が行う手順ではないということで、「その他運営全般に関わるマニュアル」に書くことにしました。
## はじめに
秀和システムが出版している『親子で電子工作入門&nbsp;ラズパイとスマホでラジコン戦車を作ろう！』をラズパイゼロで動かしてやろうというのが、この章での趣旨です。

この本では、Raspberry Pi 3 を使ったラジコン戦車（本の中ではRC戦車） の作り方や、作った戦車で行う実験などを、初心者向けにわかりやすく紹介しています。 この本で作るラジコン戦車は、スマホと Raspberry Pi 3 をBluetooth接続して、2つのモータの制御をしたり、ラジコン戦車に付けられた加速度センサからの情報をもとに、車体の傾きや速度を知ることができます。

初心者でも簡単に作れるように、ラジコン戦車を動かすためのプログラムは、自分で作る必要がなく、サポートページからイメージファイルをダウンロードして、Micro SDカードに書き込めばいいようになっています。

このラジコン戦車のキットですが、Raspberry Pi3 が付属しているセットがamazonにて販売されています。

<!--ラジコン戦車を作りたいだけなのに、無駄にスペックが高い部品ばかりな気がします。そもそも、これくらいのプログラムなら[ラズパイ3](https://a.r10.to/hboCpt)を使わずとも[ラズパイゼロ](https://www.amazon.co.jp/gp/product/B076BHY12Y/ref=as_li_tl?ie=UTF8&camp=247&creative=1211&creativeASIN=B076BHY12Y&linkCode=as2&tag=aikotobahaabu-22&linkId=38e5828a1a3829ffe12b54adf5833df5)で十分動くわけです。-->
ラジコン戦車は、ラズパイゼロでも動かすことが出来ます。 ラズパイゼロを使えば消費電力は小さく、購入価格も安く済むので、バッテリも小さくて安いもので十分動作するわけです。

ということで、[ラズパイゼロ](https://www.amazon.co.jp/gp/product/B076BHY12Y/ref=as_li_tl?ie=UTF8&camp=247&creative=1211&creativeASIN=B076BHY12Y&linkCode=as2&tag=aikotobahaabu-22&linkId=38e5828a1a3829ffe12b54adf5833df5)で動かしてみました。

とはいうものの、単純に、この本に書いてあるようにサポートページから入手したイメージファイルをインストールしても、ラズパイゼロは起動すらしませんでした。これはイメージファイル作成時のRaspbianのバージョンが古く、ラズパイゼロとの互換性がないからでしょう。しかたがないので、最新バージョンのRaspbianである、Raspbian Busterをラズパイゼロにインストールして動かしてみました。

## ラズパイゼロの初回設定をするにあたって必要なもの

[元ネタの本](https://www.amazon.co.jp/gp/product/4798050202/ref=as_li_tl?ie=UTF8&tag=aikotobahaabu-22&camp=247&creative=1211&linkCode=as2&creativeASIN=4798050202&linkId=7b732fafe7316d1c4de4e3d82620f6e5)では、サポートページから入手したイメージファイルをインストールすればラジコン戦車を動かせたので、マウスやキーボードなど、周辺機器を準備する必要がありませんでした。今回、ラズパイゼロで動かすにあたって、ラジコン戦車のプログラムを入れたりする必要があるため、元ネタの本に書いてあるもの以外にも用意する必要があります。基本的には、スターターキットにあるものと、マウスなどの周辺機器を用意すればいいです。  
スターターキットにあるもの以外には、HDMIケーブル、マウス、キーボード、ディスプレイといったとこでしょうか。このスターターキットでは、Micro-SDカードに既にRaspbianが書き込まれているので、次の項は飛ばして構いません。

##  Raspbianのインストール

ここでは、元ネタの本のように、Raspberry Piの公式ホームページから[Raspbian](https://www.raspberrypi.org/downloads/raspbian/)をダウンロードし、 Micro-SDカードに書き込みました。

以下のサイトのようにNOOBSでインストールすることもできます。 NOOBSでインストールすれば、インストールに時間がかかりますが、操作がわかりやすいですし、「Win32 Disk Imager」のようなソフトは必要ありません。

https://qiita.com/henjiganai/items/2b39dd8cbc3cc3cbe4a4

## 初期設定ツールでの設定

インストールできたらターミナルを開き、以下のコマンドを入力します。

    raspi-config

「 Advanced Options 」から「 Expand Filesystem 」を選択します。

![Expand Filesystem](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-10-093655_1824x984_scrot.png)

今回のラジコン戦車ではI2C通信を使うため、「I2C」を有効にします。

![i2c](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-10-093855_1824x984_scrot2.png)

デスクトップにあるスタートボタンから、設定→Raspberry Piの設定と進み、ホスト名を変更します。

![host name](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-10-100614_1824x984_scrot2-1.png)

ここで一旦再起動しておきます。

ここからはBluetoothサービスの設定を行っていきます。

## Bluetoothのインストール

Bluetoothサービスをインストールします。

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get install bluetooth

Bluetoothサービスの設定ファイルに変更を加えます。

    sudo nano /etc/systemd/system/dbus-org.bluz.service

「ExecStart」が書かれている行に

    --compat

を付け加えます。

![](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-10-094541_1824x984_scrot2-1.png)

hci0を有効にします。

    sudo hciconfig hci0 up

hci0が起動時に自動的に有効になるファイルを新しく作成します。

    sudo nano /etc/udev/rules.d/10-local.rules

以下のように記述します。

    # Set bluetooth power up
    ACTION=="add", KERNEL=="hci0", RUN+="/usr/bin/hciconfig hci0 up"

![](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-10-095410_1824x984_scrot2.png)

Bluetoothを常に待ち受け状態にします。

    sudo nano /etc/bluetooth/main.conf

「DiscoverableTimeout」と「PairrableTimeout」の行の「#」を削除します。

![](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-10-095511_1824x984_scrot2-1.png)

次はラジコン戦車を動かすプログラムをラズパイに入れたいと思います。

まず、ラジコン戦車を動かすプログラムやそれを自動起動させるシェルスクリプトが入っているフォルダをサポートページのイメージファイルから抜き出します。そのために[サポートページ](https://www.shuwasystem.co.jp/support/7980html/5020.html#1)にアクセスし、「Raspberry Pi 3に指すSDカードメモリのイメージファイル」をダウンロードします。

ダウンロードしたファイルは圧縮されたイメージファイルですが、[7-zip](https://sevenzip.osdn.jp/)を使えばその中のファイルやフォルダを抜き出すことができます。

7-zipを起動して、「RemoteControl」のフォルダを見つけ出し、USBメモリなどにコピペします。 「RemoteControl」  のフォルダは

    \RasPiTank_rasbian20170415.zip\RasPiTank_rasbian20170415.img\1.img\home\pi

の中にあります。

![](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-24-1-1024x601.png)

この 「RemoteControl」のフォルダをラズパイゼロに移します。移す場所は\home\pi下です。

次に「RemoteControl」のフォルダの中にある「TankControl.c」のファイルをコンパイルし直します。まずは/home/pi/RemoteControlのディレクトリに移動します。

    cd /home/pi/RemoteControl

「make clean」でRemoteControlの中にある実行ファイルなどを削除します。

    make clean

「make」で再度、実行ファイルを作成します。

    make

これでラズパイゼロに対応したラジコン戦車の実行ファイルができあがりました。

次にラジコン戦車プログラムを起動させるシェルスクリプトに変更を加えます。bluetoothctlコマンドがラズパイゼロだと実行されるまで時間がかかるようで、変更しないと動作しませんでした。試行錯誤の末、「RemoteControl」の中にある「 register_rfcomm.sh 」のbluetoothctlの行の後に、sleepを入れることで動作しました。

    #!/bin/sh

    bluetoothctl -a NoInputNoOutput&
    sleep 5
    sdptool add --channel=7 SP
    /home/pi/RemoteControl/TankControl&
    hciconfig hci0 piscan

    while true
        do
    	rfcomm listen /dev/rfcomm0 7
    done

本の中では書かれていなかったのですが、シェルスクリプトは動作させるのに権限を与える必要があります。

    chmod 755 /home/pi/RemoteControl/register_rfcomm.sh

このシェルスクリプトを自動起動させるためにrc.localファイルに明記しました。「fi」と「exit 0」の間に以下を記述します。

    /home/pi/RemoteControl/register_rfcomm.sh &

![](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-10-100452_1824x984_scrot2-1.png)

再起動すれば完了です。

    sudo reboot

## 動くか確認

ラジコン戦車の車体と繋げて動くか確認します。ラズパイゼロのピン配列はラズパイ3と同じになっているので、本に書いてあるように繋げればOKです。

![](http://hara.jpn.com/images/_default/Topics/RaspPiZero/RaspPiZero.png)


実際に動かしてみた動画です。小さいバッテリでも動いてます。

[![](https://img.youtube.com/vi/HA8JyUupVh0&feature=emb_title&ab_channel=Amplil/0.jpg)](https://www.youtube.com/watch?v=HA8JyUupVh0&feature=emb_title&ab_channel=Amplil)
