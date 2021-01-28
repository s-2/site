# build a ffh gluon based on the `next` branch (OpenWrt master)

``` shell
# clone gluon
git clone https://github.com/freifunk-gluon/gluon -b next
cd gluon

# clone site conf
git clone https://github.com/freifunkh/site -b next

# apply site patches
git am site/patches/*

# if you want to test packages in ffh-packages, you
# need to adjust PACKAGES_HANNOVER_COMMIT to the
# lastest commit id in site/modules now
vi site/modules

# if you want to bump up the version number
vi site/site.mk

mkdir logs

make update

# maybe switch here to a screen session ;)
make V=s GLUON_TARGET=ar71xx-generic &> logs/$(date -Is)_ar71xx-generic.log

```

# build all targets

``` shell
TARGETS="ath79-generic brcm2708-bcm2708 brcm2708-bcm2709 brcm2708-bcm2710 ipq40xx-generic ipq806x-generic lantiq-xrx200 lantiq-xway mpc85xx-generic mpc85xx-p1020 mvebu-cortexa9 ramips-mt7620 ramips-mt7621 ramips-mt76x8 ramips-rt305x sunxi-cortexa7 x86-64 x86-generic x86-geode"

for t in $TARGETS; do
  make V=s GLUON_TARGET=${t} &> logs/$(date -Is)_${t}.log
done
```

# build for debugging purposes

- Debugging symbols are not stripped while building. (At least you still need to compile the package with `-g` to get the debug symbols, but they are not stripped)
- Packages `gdb`, `valgrind` and `strace` will be built in the image

``` shell
make V=s GLUON_DEBUG=1 GLUON_TARGET=x86-generic &> logs/$(date -Is)_x86-generic.log
```
