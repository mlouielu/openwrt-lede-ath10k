OpenWrt / LEDE ath10k development
=================================

This is a note about how I do some research and develop about ath10k driver on OpenWrt / LEDE platfrom.

## HOW TO USE

You should read through blog post to make sure you have the basic knowledge about
develop process and how to build the images, kernel modules.


## BLOG POST

* [Build LEDE for TP-Link Archer C2600 from source (cht)](https://blog.louie.lu/2017/09/25/build-lede-for-tp-link-archer-c2600-from-source/)
* [Building LEDE / OpenWrt for TP-Link C2600 with ftrace enable](https://blog.louie.lu/2017/12/28/building-lede-openwrt-tp-link-c2600-ftrace-enable/)
* [Develop process in Linux ath10k driver with LEDE](https://blog.louie.lu/2018/01/10/develop-process-linux-ath10k-driver-lede/)

## TEST TOOLS

* [flent](https://github.com/tohojo/flent)
* [irtt](https://github.com/peteheist/irtt)
* [udper](https://github.com/mlouielu/udper)
* [iperf3](https://github.com/esnet/iperf)
* [wireshark](https://github.com/wireshark/wireshark)

## REPOs

* [ath - priv repo](https://bitbucket.org/mlouielu/ath)
* [lede](https://github.com/lede-project/source)

## Explain

### ath - priv repo

* vo_queue_tv: this branch will ensure dscp with 0xE0 (224) packet to send the
packet when it get into the driver, no matter other previous packet in the driver queue list.
* priority_station: this branch will ensure the setting station's packet to send
the packet when it get into the driver, no matter other previous packet in the driver queue list.

### Testing

#### flent

This is a powerful tools that conbime many test case to test. It has been used on
the academic paper and be accpeted by the proceedings.

Some useful test will be:

##### 60 seconds voip test

```
$ flent voip -l 60 -H 192.168.166.33
```

##### 60 seconds voip-rrul (voip realtime response under load)

```
$ flent voip-rrul -l 60 -H 192.168.166.33
```

##### 60 seconds rtt-fair-var (round trip time fairness test)

```
$ flent rtt_fair_var -l 60 -H 192.168.166.33 -H 192.168.166.34
```

#### irtt

IRTT measures round-trip time, one-way delay and other metrics using UDP packets
sent on a fixed period, and produces both user and machine parseable output.

It is used to replace D-ITG testing tools, which is a standard in voip test but
hard to used.

```
$ irtt client -i 20ms -l 172 -d 30s --fill=rand --sfill=rand --dscp=0x0 192.168.166.33
$ irtt client -i 20ms -l 172 -d 30s --fill=rand --sfill=rand --dscp=0xE0 192.168.166.33
```

#### udper

This is a small scripts to test the UDP packet order.
