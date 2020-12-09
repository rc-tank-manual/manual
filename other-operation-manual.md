# ラズパイゼロでラジコン戦車プログラムを動かす方法
これは私のブログ( http://electronic-handicraft.work )にある内容を書いたものです。

まず、ラズパイゼロやその他周辺機器を用意して、 実際にラズパイゼロを動かしてみます。

## ラズパイゼロの初回設定をするにあたって必要なもの

[元ネタの本](https://www.amazon.co.jp/gp/product/4798050202/ref=as_li_tl?ie=UTF8&tag=aikotobahaabu-22&camp=247&creative=1211&linkCode=as2&creativeASIN=4798050202&linkId=7b732fafe7316d1c4de4e3d82620f6e5)では、サポートページから入手したイメージファイルをインストールすればラジコン戦車を動かせたので、マウスやキーボードなど、周辺機器を準備する必要がありませんでした。今回、ラズパイゼロで動かすにあたって、ラジコン戦車のプログラムを入れたりする必要があるため、元ネタの本に書いてあるもの以外にも用意する必要があります。基本的には、スターターキットにあるものと、マウスなどの周辺機器を用意すればいいです。  
スターターキットにあるもの以外には、HDMIケーブル、マウス、キーボード、ディスプレイといったとこでしょうか。このスターターキットでは、Micro-SDカードに既にRaspbianが書き込まれているので、次の項は飛ばして構いません。

##  Raspbianのインストール

ここでは、元ネタの本のように、Raspberry Piの公式ホームページからRaspbianをダウンロードし、 Micro-SDカードに書き込みました。

[Raspbian](https://www.raspberrypi.org/downloads/raspbian/)

以下のサイトのようにNOOBSでインストールすることもできます。 NOOBSでインストールすれば、インストールに時間がかかりますが、操作がわかりやすいですし、「Win32 Disk Imager」のようなソフトも必要ありません。

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

今回は以上です。次回からBluetoothサービスの設定を行っていきます。

[前回](http://electronic-handicraft.work/?p=65)はラズパイゼロの初回設定を行いました。今回はBluetoothサービスの設定を行っていきます。

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

今回は以上です。次回からラジコン戦車を動かすプログラムをラズパイゼロに入れていきたいと思います。

[前回](http://electronic-handicraft.work/?p=79)はBluetoothサービスの設定を行いました。今回は ラジコン戦車を動かすプログラムをラズパイに入れたいと思います。

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

本の中では何も書かれていなかったのですが、シェルスクリプトは権限を与えないと動作しないと思います。

    chmod 755 /home/pi/RemoteControl/register_rfcomm.sh

このシェルスクリプトを自動起動させるために、本とは違い、rc.localファイルに明記しました。「fi」と「exit 0」の間に以下を記述します。

    /home/pi/RemoteControl/register_rfcomm.sh &

![](http://electronic-handicraft.work/wordpress/wp-content/uploads/2019/07/2019-07-10-100452_1824x984_scrot2-1.png)

再起動すれば完了です。

    sudo reboot

### 動くか確認

ラジコン戦車の車体と繋げて動くか確認します。ラズパイゼロのピン配列はラズパイ3と同じになっているので、本に書いてあるように繋げればOKです。

![](http://hara.jpn.com/images/_default/Topics/RaspPiZero/RaspPiZero.png)


実際に動かしてみた動画です。小さいバッテリでも動いてます。

[![](https://img.youtube.com/vi/HA8JyUupVh0&feature=emb_title&ab_channel=Amplil/0.jpg)](https://www.youtube.com/watch?v=HA8JyUupVh0&feature=emb_title&ab_channel=Amplil)
