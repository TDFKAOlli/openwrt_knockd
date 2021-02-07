# Knockd for OpenWrt/LEDE

Tested with LEDE v18.07.6 for arm_cortext_a9

Built from latest git version 0.8.2 from TDFKAOlli fork.

Be advised that the upstream project is more or less unmaintained. This does _not_ mean you can't use knockd in production, but to think about it carefully. But if other, modern portknocking solutions feel too cumbersome for you, knockd may exactly be what you need.

## About

This is an updates package source for building the latest stable version of knockd for OpenWrt/LEDE.

Initscript and logbuffer compatible config are included.

## Precompiled packages
Available [here](https://github.com/milaq/openwrt_knockd/releases/)

## Building

Install prerequisites (for _Debian_):
````
apt-get install build-essential libncurses5-dev gawk git subversion libssl-dev gettext unzip zlib1g-dev file python curl
````

Get the LEDE source and checkout the latest revision:
````
git clone https://git.lede-project.org/source.git lede
cd lede
git checkout v18.07.6
````

Get package for knockd:
````
git clone https://github.com/TDFKAOlli/openwrt_knockd.git package/knockd
````

Move to the branch which will fetch the latest knockd from TDFKAOlli repository:
````
cd package/knockd
git checkout fetch_knockd_from_tdfkaolli
````

Build toolchain:
When initially building select your target system and make sure `Network` -> `Firewall` -> `knockd` is selected.
````
make tools/install
make toolchain/install
````

In case you didn't built the whole tree before you need to compile libpcap:
````
make package/libs/libpcap/configure
make package/libs/libpcap/compile
````

Build the package:
````
make package/knockd/clean
make package/knockd/download
make package/knockd/compile
````

Get the built ipk for your target:
````
bin/packages/<VERSION>/base/knockd_0.8.2-3_<VERSION>.ipk
````
Example for arm_cortex-a9_vfpv3-d16:
````
bin/packages/arm_cortex-a9_vfpv3-d16/base/knockd_0.8.2-3_arm_cortex-a9_vfpv3-d16.ipk
````

## Installing

Install the ipk:
Scp the ipk to `/tmp` on your LEDE machine and issue a
````
opkg install /tmp/knockd_0.8.2-3_<VERSION>.ipk
````

Change the default configuration:
````
/etc/config/knockd
````

Start and enable the deamon:
````
/etc/init.d/knockd start
/etc/init.d/knockd enable
````
