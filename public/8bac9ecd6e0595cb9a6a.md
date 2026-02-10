---
title: ASUSのルーターのCLIで遊ぶ
tags:
  - Network
  - 通信
  - ルータ
  - Router
  - ルーター
private: false
updated_at: '2025-12-04T19:24:09+09:00'
id: 8bac9ecd6e0595cb9a6a
organization_url_name: null
slide: false
ignorePublish: false
---
## はじめに
ASUSのルーターは, CLIでログインできる.
個人的に, よく使うコマンドや参照するファイルをまとめる.

:::note info
CLIログインの有効化の方法は, 以下の公式FAQを参照
:::

https://www.asus.com/jp/support/FAQ/1048201

## 確認機種

https://www.asus.com/jp/Networking/RT-AX3000

<img src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/56d2a541-dfaf-cb15-a1b0-0746780d1ba5.png" width="300px" alt=rt-ax3000>

:::note info
管理画面は, 以下のASUS公式でデモがあるので, 試しに触ることができる. 
:::

https://demoui.asus.com/JP/

![nw_map.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/cbb6ce59-f8ad-c5bb-8746-15b37b74e639.png)


## 基本

- NVRAMに保存するパラメータ一覧を表示

```sh
# nvram show
```

なお, JFFS (`/jffs/nvram`) のディレクトリ配下でも確認できる.

## ネットワーク

[管理画面で確認できる接続 (NAT) 履歴](https://demoui.asus.com/JP/Main_ConnStatus_Content.asp) は, CLIでは `/tmp/connect.log` で確認できる.

![nat.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/0153ca52-51c6-9902-0a65-37f2c81204e1.png)


## ワイヤレス

https://wiki.dd-wrt.com/wiki/index.php/Wl_command

`wl`コマンドを使う.

```sh
# wl status
SSID: "skkzsh12ap"
Mode: Managed   RSSI: 0 dBm     SNR: 0 dB       noise: -87 dBm  Channel: 10
BSSID: AA:BB:CC:DD:EE:FF        Capability: ESS ShortSlot RRM
Supported Rates: [ 1(b) 2(b) 5.5(b) 6 9 11(b) 12 18 24 36 48 54 ]
HE Capable:
        Chanspec: 2.4GHz channel 10 20MHz (0x100a)
        Primary channel: 10
        HT Capabilities: 40MHz SGI20 SGI40
        Supported HT MCS : 0-15
        Supported VHT MCS:
                NSS1 Tx: 0-11        Rx: 0-11
                NSS2 Tx: 0-11        Rx: 0-11
        Supported HE MCS:
            20/40/80 MHz:
                NSS1 Tx: 0-11        Rx: 0-11
                NSS2 Tx: 0-11        Rx: 0-11
```

## DHCP

- [DHCP固定割当IPアドレス](https://demoui.asus.com/JP/Advanced_DHCP_Content.asp) の一覧を確認

```sh
# nvram get dhcp_staticlist
```

また, 以下のようにプロセスを確認すると, dnsmasqを使用していることが分かる.

```sh
# ps | grep dns | grep -v grep
 1359 nobody    2380 S    dnsmasq --log-async
```

https://wiki.dd-wrt.com/wiki/index.php/DNSMasq_as_DHCP_server

よって, [DHCPの設定](https://demoui.asus.com/JP/Advanced_DHCP_Content.asp) は `/etc/dnsmasq.conf` で確認できる.
[DHCPリースの状態 (リース時間, MACアドレス, IPアドレス, ホスト名)](https://demoui.asus.com/JP/Main_DHCPStatus_Content.asp) は `/var/lib/misc/dnsmasq.leases` で確認できる.

![dhcp_lease.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/6361/2810ae2a-84bd-7020-4374-ad219af7837b.png)


## IPv6

- [接続しているクライアントの一覧 (ホスト名, MACアドレス, IPv6アドレス)](https://demoui.asus.com/JP/Main_IPV6Status_Content.asp) は `/tmp/ipv6_client_info` や `/tmp/ipv6_client_list` で確認できる.

- [ファイアウォールの設定](https://demoui.asus.com/JP/Advanced_BasicFirewall_Content.asp) を確認

```sh
# nvram get ipv6_fw_rulelist
```

## デーモン

```sh
# ps | grep :22 | grep -v grep
 1282 nobody    2312 S    dropbear -p 192.168.50.1:22 -a
```

より, SSHサーバはdropbearを使っていることが分かる.



