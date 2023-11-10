# Zeek-Parser-CCLinkTSNSLMP

English is [here](https://github.com/nttcom/zeek-parser-CCLinkTSNSLMP/blob/main/README_en.md)

## 概要

Zeek-Parser-CCLinkTSNSLMPとは[CC-Linkファミリー](https://www.cc-link.org/ja/cclink/index.html)のCC-Link IE TSNのIPフレームを解析できるZeekプラグインです。

## インストール

### マニュアルインストール

本プラグインを利用する前に、Zeek, Spicyがインストールされていることを確認します。

```
# Zeekのチェック
~$ zeek -version
zeek version 5.0.0

# Spicyのチェック
~$ spicyz -version
1.3.16
~$ spicyc -version
spicyc v1.5.0 (d0bc6053)

# 本マニュアルではZeekのパスが以下であることを前提としています。
~$ which zeek
/usr/local/zeek/bin/zeek
```

本リポジトリをローカル環境に `git clone` します。

```
~$ git clone https://github.com/nttcom/zeek-parser-CCLinkTSNSLMP.git
```

## 使い方

### マニュアルインストールの場合

ソースコードをコンパイルして、オブジェクトファイルを以下のパスにコピーします。

```
~$ cd ~/zeek-parser-CCLinkTSNSLMP/analyzer
~$ spicyz -o cc_link_tsn_slmp.hlto cc_link_tsn_slmp.spicy cc_link_tsn_slmp.evt
# cc_link_tsn_slmp.hltoが生成されます
~$ cp cc_link_tsn_slmp.hlto /usr/local/zeek/lib/zeek-spicy/modules/
```

同様にZeekファイルを以下のパスにコピーします。

```
~$ cd ~/zeek-parser-CCLinkTSNSLMP/scripts/
~$ cp main.zeek /usr/local/zeek/share/zeek/site/cc_link_tsn_slmp.zeek
```

最後にZeekプラグインをインポートします。

```
~$ tail /usr/local/zeek/share/zeek/site/local.zeek
...省略...
@load cc_link_tsn_slmp
```

本プラグインを使うことで `cclink-ie-tsn-slmp.log` が生成されます。

```
~$ cd ~/zeek-parser-CCLinkTSNSLMP/testing/Traces
~$ zeek -Cr test.pcap /usr/local/zeek/share/zeek/site/cc_link_tsn_slmp.zeek
```

## ログのタイプと説明

本プラグインはCC-Link IE TSNのIPフレームの全ての関数を監視して`cclink-ie-tsn-slmp.log`として出力します。

| フィールド | タイプ | 説明 |
| --- | --- | --- |
| ts | time | 最初に通信した時のタイムスタンプ |
| uid | string | ユニークID |
| id.orig_h | addr | 送信元IPアドレス |
| id.orig_p | port | 送信元ポート番号 |
| id.resp_h | addr | 宛先IPアドレス |
| id.resp_p | port | 宛先ポート番号 |
| service | string | プロトコル名 |
| flame_type | string | データフレームの名前 |
| pdu_type | string | プロトコルの関数名 |
| cmd | string | コマンド、サブコマンドにより局に対する操作を指定する |
| number | int | パケット出現回数 |
| ts_end | time | 最後に通信した時のタイムスタンプ |

`cclink-ie-tsn-slmp.log` の例は以下のとおりです。

```
#separator \x09
#set_separator	,
#empty_field	(empty)
#unset_field	-
#path	cclink-ie-tsn-slmp
#open	2023-11-08-08-01-46
#fields	ts	uid	id.orig_h	id.orig_p	id.resp_h	id.resp_p	service	flame_type	pdu_type	cmd	number	ts_end
#types	time	string	addr	port	addr	port	string	string	string	string	int	time
1695784130.196558	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-notification	ResNotification	10	1695784186.147303
1695784131.117451	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-networkConfigTslt	ReqNetworkConfigTslt	10	1695784187.077320
1695784131.151672	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-slaveConfig	ReqSlaveConfig	10	1695784187.112323
1695784130.563997	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-cyclicConfigRcvSrcInfo	ReqCyclicConfigRcvSrcInfo	10	1695784186.521313
1695784130.563923	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-cyclicConfigTrnSubPayload	ReqCyclicConfigTrnSubPayload	10	1695784186.521220
1695784130.563802	CeiMXE22eYUYMBHZsj	10.0.0.1	45238	10.0.0.2	45238	cclink_ie_tsn	ip	slmp-cyclicConfigMain	ReqCyclicConfigMain	10	1695784186.521153
#close	2023-11-08-08-01-46
```

## 関連ソフトウェア

本プラグインは[OsecT](https://github.com/nttcom/OsecT)で利用されています。
