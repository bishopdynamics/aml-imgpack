aml-imgpack
===========

Resource packer/unpacker for Amlogic Logo image files, updated for python3

About bootup.bmp
----------------

Make sure you respect the original image sizes and depth, or else u-boot might get confused.

For instance, here are some example images from Spotify Car Thing (superbird):
```bash
bad_charger.bmp:       PC bitmap, Windows 98/2000 and newer format, 480 x 800 x 16, cbSize 768138, bits offset 138
bootup.bmp:            PC bitmap, Adobe Photoshop with alpha channel mask, 360 x 360 x 16, cbSize 259272, bits offset 70
bootup_spotify.bmp:    PC bitmap, Windows 98/2000 and newer format, 480 x 800 x 16, cbSize 768138, bits offset 138
upgrade_bar.bmp:       PC bitmap, Adobe Photoshop with alpha channel mask, 4 x 14 x 16, cbSize 184, bits offset 70
upgrade_error.bmp:     PC bitmap, Windows 98/2000 and newer format, 480 x 800 x 16, cbSize 768138, bits offset 138
upgrade_fail.bmp:      PC bitmap, Adobe Photoshop with alpha channel mask, 300 x 300 x 16, cbSize 180072, bits offset 70
upgrade_logo.bmp:      PC bitmap, Adobe Photoshop with alpha channel mask, 300 x 300 x 16, cbSize 180072, bits offset 70
upgrade_success.bmp:   PC bitmap, Windows 98/2000 and newer format, 480 x 800 x 16, cbSize 768138, bits offset 138
upgrade_unfocus.bmp:   PC bitmap, Adobe Photoshop with alpha channel mask, 4 x 14 x 16, cbSize 184, bits offset 70
upgrade_upgrading.bmp: PC bitmap, Adobe Photoshop with alpha channel mask, 300 x 300 x 16, cbSize 180072, bits offset 70
```

Images will always be centered when shown on the screen, so you can make actual images only as large as necessary.

You can see all images are 16bit. Using GIMP: File -> Export, Advanced Options, `16bits R5 G6 B5`.

Help
----

```
$ ./aml-imgpack.py --help
usage: aml-imgpack.py [-h] [--unpack] [--pack PACK] file [file ...]

Pack and unpack amlogic uboot images

positional arguments:
  file         an integer for the accumulator

optional arguments:
  -h, --help   show this help message and exit
  --unpack     Unpack image file
  --pack PACK  Pack image file
```

Listing assets in an image
--------------------------

```
$ ./aml-imgpack.py logo.dump
Listing assets in logo.dump
AmlResImgHead(crc=0x6a314443 version=2 imgSz=2697056 imgItemNum=10 alignSz=16)
    AmlResItem(name=upgrade_unfocus start=0x2c0 size=184)
    AmlResItem(name=upgrade_success start=0x380 size=180072)
    AmlResItem(name=upgrade_logo start=0x2c2f0 size=180072)
    AmlResItem(name=upgrade_error start=0x58260 size=180072)
    AmlResItem(name=bootup start=0x841d0 size=259272)
    AmlResItem(name=upgrade_upgrading start=0xc36a0 size=180072)
    AmlResItem(name=bootup_spotify start=0xef610 size=768138)
    AmlResItem(name=upgrade_bar start=0x1aaea0 size=184)
    AmlResItem(name=upgrade_fail start=0x1aaf60 size=180072)
    AmlResItem(name=bad_charger start=0x1d6ed0 size=768138)
```

Unpacking an image
------------------

When unpacking, asset names are appended with `.bmp` extension to make editing easier.

```
$ ./aml-imgpack.py --unpack logo.img
Unpacking assets in logo.img
  Unpacking upgrade_unfocus
  Unpacking upgrade_success
  Unpacking upgrade_error
  Unpacking upgrade_fail
  Unpacking bootup
  Unpacking upgrade_bar
  Unpacking upgrade_logo
  Unpacking upgrade_upgrading
```

Packing an image
----------------

When packing, `.bmp` will be stripped from the asset name.

```
$ python aml-imgpack.py --pack out.img *.bmp
Packing files in out.img:
  bootup (259272 bytes)
  upgrade_bar (184 bytes)
  upgrade_error (180072 bytes)
  upgrade_fail (180072 bytes)
  upgrade_logo (180072 bytes)
  upgrade_success (180072 bytes)
  upgrade_unfocus (184 bytes)
  upgrade_upgrading (180072 bytes)
```
