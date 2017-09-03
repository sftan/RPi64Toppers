# RPi64Toppers

Raspberry Pi 64bitモード向けTOPPERSリアルタイムカーネル

## 概要

 - TOPPERS/FMPカーネル(https://www.toppers.jp)を移植
 - Raspberry Pi 3のARMv8 AArch64モード
 - 4コアで動作
 - キャッシュ，MMUはON
 - 各コアのARM Generic Timerを使用
 - Raspberry PiのMini UARTを使用
 - 例外レベル3(EL3)でブートし，例外レベル1ノンセキュア(EL1NS)でカーネル動作

## 必要な機材

 - Raspberry Pi 3 + 電源
 
 - SDカード  
 FAT32フォーマットされたSDカードを用意する  
 数十MB程度の空きがあれば十分である
 
 - USBシリアル変換ケーブル  
 シリアルコンソールで通信するために使用する  
 TTL-232R-3V3を使用した  

 - Windows PC
 Cygwinを用いてビルドを行う
 説明は省くがLinux PCでも可

## 開発環境の構築

 - Cygwin
 WindwosPCにCygwinをインストールする
 おそらく以下パッケージが必要
 make, perl, git, gcc-core, gcc-g++

 - コンパイラ  
 以下からフリーのAArch64用コンパイラを入手する  
 https://www.linaro.org/downloads/  
 開発ではWindows用の以下Versionのものを使用した  
 gcc-linaro-6.3.1-2017.02-i686-mingw32_aarch64-elf  
 DownLoadして適当な場所に解凍したら
 ```<解凍したディレクトリ>/gcc-linaro-6.3.1-2017.02-i686-mingw32_aarch64-elf/bin/aarch64-elf-gcc```
 にpathに通す

 - ターミナルアプリ
 シリアルコンソールで通信するためにTera Termなどをインストールしておく

## ビルド

このリポジトリを取得してfmp.binをビルドする
```
git clone https://github.com/YujiToshinaga/RPi64Toppers.git
cd RPi64Toppers/fmp
mkdir test
cd test
perl ../configure -T rpi_arm64_gcc
make fmp.bin
```

## 動作準備

### SDカードの準備

FAT32フォーマットされたSDカード直下に  
以下の4ファイルを置いてRaspberry Piに挿す

 - bootcode.bin, start.elf  
 以下からbootcode.binとstart.elfをダウンロードする  
 https://github.com/raspberrypi/firmware/tree/master/boot

 - config.txt  
 このリポジトリからconfig.txtを取得する
 
 - fmp.bin  
 ビルドしたfmp.binを使用する

### シリアルコンソールの接続

Raspberry PiのTXD1, RXD1をシリアルデバイスと接続する  
ボーレートは115200bpsにする

TTL-232R-3V3を使用する場合

RPiのピン | 結線 | TTL-232R-3V3のピン
---|---|---
GPIO14(TXD1) | - | Yellow(RXD)
GPIO15(RXD1) | - | Orange(TXD)
Ground | - | Ground |

参考：Raspberry Piのピン配置
https://www.raspberrypi.org/documentation/usage/gpio-plus-and-raspi2/README.md

参考：TTL-232R-3V3の概要
http://www.ftdichip.com/Support/Documents/DataSheets/Cables/DS_TTL-232R_CABLES.pdf

## 起動

Raspberry Piの電源を入れる

