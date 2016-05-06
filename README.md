About GDP: GENIVI Development Platform
================================

GENIVI Development Platform is the integration and delivery project that brings together all components developed by GENIVI experts and provides them to developers and users in a consumable way. You can find all the relevant information about GDP in the wiki:
* GDP [landing page](https://projetcs.genivi.org/gdp)
* GDP [Download page](https://projetcs.genivi.org/gdp/download)
* GDP [Roadmap](https://projetcs.genivi.org/gdp/roadmap)
* GDP-ivi9 [feature page](https://projetcs.genivi.org/gdp/gdp9)
* GDP-ivi9 [ports to target boards](https://at.projects.genivi.org/wiki/display/GDP/GDP+target+boards%2C+virtualization+and+peripherals)
* [GDP In Detail](https://at.projects.genivi.org/wiki/pages/viewpage.action?pageId=11567879) with further information about GDP components.
* [GENIVI on GitHub](https://github.com/GENIVI)
* Information about how [GDP is managed](https://at.projects.genivi.org/wiki/display/GDP/GENIVI+Development+Platform+management)

Contribute to GDP
-----------------------

Please see the  [MAINTAINERS](https://github.com/genivi/meta-genivi-dev/blob/master/MAINTAINERS)
file for information on contacting the maintainers of this layer, as well as instructions for submitting patches.

The GENIVI Development Platform project welcomes contributions. You can contribute
code, submit patches, report bugs, answer questions on our mailing lists and
review and edit our documentation and much more.

Subscribe to the mailing list
    [here](https://lists.genivi.org/mailman/listinfo/genivi-projects).
IRC Channel
    #automotive in freenode.net
View or Report bugs
    [GENIVI uses JIRA as bug tracker and task management tool](https://at.projects.genivi.org/jira/projects/GDP/issues).
For information about the Yocto Project, see the
    [Yocto Project website](https://www.yoctoproject.org).  
For information about the Yocto GENIVI Baseline, see the
    [Yocto GENIVI Baseline website](http://projects.genivi.org/GENIVI_Baselines/meta-ivi).

meta-genivi-dev: the Yocto layer for the GENIVI Development Platform
=====================================================

This layer provides a GENIVI Development Platform (GDP) image build. The layer
supports cross-architecture application development using QEMU emulation and an SDK.

Building the GENIVI Development Platform (GDP)
-----------------------------------------------------------------
To build the GDP, GENIVI maintains a git sub-module repo with branches specific for
the supported build targets:
    [genivi-dev-platform.git](https://github.com/genivi/genivi-dev-platform/branches/all)

For example, to generate the build environment for the QEMUx86-64 target:
```bash
$ mkdir GDP
$ cd GDP
$ git clone --recursive http://github.com/genivi/genivi-dev-platform.git -b qemux86-64
$ cd genivi-dev-platform
$ source init.sh
$ bitbake genivi-dev-platform
```

More specific information on build targets, including build steps and deployment instructions
for each supported target, check [here](https://at.projects.genivi.org/wiki/display/GDP/GDP+target+boards%2C+virtualization+and+peripherals)

Layer Dependency List
------------------------------
URI: git://git.yoctoproject.org/meta-ivi
* branch:   9.0
* revision: bfd95c5021885ed61b58a33087a4ee8e3d2f32ad

URI: https://github.com/meta-qt5/meta-qt5.git
* branch:   fido
* revision: 90919b9d86988e7da01fa2c0a07246b5b5600a5d

URI: git://git.openembedded.org/meta-openembedded
* layers:   meta-oe, meta-ruby, meta-filesystems
* branch:   fido
* revision: a7c1a2b0e6947740758136216e45ca6ca66321fc

URI: git://git.yoctoproject.org/poky
* branch:   fido
* revision: 900d7d6b59c36b2bdbd1c85febec99e80ab54f95

## The Raspberry Pi2 board depend in addition on: ##

URI: git://git.yoctoproject.org/meta-raspberrypi
* branch:   master
* revision: 519c387e3b97ecc21ac1d7b4fc9197298f289a71

## The Renesas R-Car Gen2 Koelsch & Porter boards depend in addition on: ##
URI: git://github.com/slawr/meta-renesas.git
* branch:   experimental-genivi-9-bsp-1.10.0-weston-1.9.0
* revision: 565dd7def3cd39724169939c68bb748f5888cafb

## The Intel Minnowboard MAX depends in addition on: ##
URI: git://git.yoctoproject.org/meta-intel
* branch: fido

Supported Machines on GDP-ivi9
--------------------------------------------
GDP-ivi9 supports the builds for these machines:

* QEMU (x86-64)                  - machine: qemux86-64
* Renesas R-Car Gen2 (R-Car M2)  - machine: porter
* Raspberry Pi 2                 - machine: raspberrypi2

Miscellaneous
-------------
When building for raspberrypi2, add the following to your local.conf:
> MACHINE = "raspberrypi2"
> GPU_MEM = "128"
> KERNEL_DEVICETREE = "bcm2709-rpi-2-b.dtb"
> PREFERRED_VERSION_linux-raspberrypi = "4.1.%"
> PREFERRED_VERSION_weston = "1.9.0"
> PREFERRED_VERSION_wayland-ivi-extension = "1.9.1"
> PREFERRED_VERSION_mesa = "11.%"
> PREFERRED_PROVIDER_virtual/egl = "mesa"
> PREFERRED_PROVIDER_virtual/libgles2 = "mesa"
> PREFERRED_PROVIDER_virtual/libgl = "mesa"
> PREFERRED_PROVIDER_virtual/mesa = "mesa"
> PREFERRED_PROVIDER_jpeg = "jpeg"

Further information about [GDP for RPi2](https://at.projects.genivi.org/wiki/display/GDP/Raspberry+Pi+2+%28RPi2%29+Hardware+Setup+and+Software+Installation)

When building for porter, add the following to your local.conf:

> MACHINE = "porter"
> LICENSE_FLAGS_WHITELIST = "commercial"
> SDKIMAGE_FEATURES_append = " staticdev-pkgs"
> MACHINE_FEATURES_append = " sgx"
> MULTI_PROVIDER_WHITELIST += "virtual/libgl virtual/egl virtual/libgles1 virtual/libgles2"
> PREFERRED_PROVIDER_virtual/libgles1 = ""
> PREFERRED_PROVIDER_virtual/libgles2 = "gles-user-module"
> PREFERRED_PROVIDER_virtual/egl = "libegl"
> PREFERRED_PROVIDER_virtual/libgl = ""
> PREFERRED_PROVIDER_virtual/mesa = ""
> PREFERRED_PROVIDER_libgbm = "libgbm"
> PREFERRED_PROVIDER_libgbm-dev = "libgbm"

Further information about GDP port to [Renesas Porter](https://at.projects.genivi.org/wiki/display/GDP/Renesas+R-Car+M2+Porter+Hardware+Setup+and+Software+Installation)


For the QEMU machine, in order to have audio, the emulation should be done like:
(please adjust to your own paths)

```bash
$ QEMU_AUDIO_DRV=pa ../../poky/scripts/runqemu ivi-image-demo qemux86-64 audio
```

Further information about [GDP for QEMU](https://at.projects.genivi.org/wiki/display/GDP/QEMU+x86_64+Hardware+Setup+and+Software+Installation)

Unsupported machines in GDP-ivi9 but supported in previous versions
-------------------------------------------------------------------------------------------

GDP supported in the past the builds for these machines. They can be supported in the future:

* Renesas R-Car Gen2 (R-Car M2)  - machine: koelsch
* Intel Minnowboard MAX (x86-64) planned to be supoprted in GDP-ivi9 - machine: minnowboard

When building for koelsch, add the following to your local.conf:

> MACHINE = "koelsch"
> USE_GSTREAMER_1_00="1"
> LICENSE_FLAGS_WHITELIST = "commercial"
> MACHINE_FEATURES_append = " sgx"
> MULTI_PROVIDER_WHITELIST += "virtual/libgl virtual/egl virtual/libgles1 virtual/libgles2"
> PREFERRED_PROVIDER_virtual/libgles1 = ""
> PREFERRED_PROVIDER_virtual/libgles2 = "gles-user-module"
> PREFERRED_PROVIDER_virtual/egl = "libegl"
> PREFERRED_PROVIDER_virtual/libgl = ""
> PREFERRED_PROVIDER_virtual/mesa = ""
> PREFERRED_PROVIDER_libgbm = "libgbm"
> PREFERRED_PROVIDER_libgbm-dev = "libgbm"

Further information about [GDP port for Koelsch]( https://at.projects.genivi.org/wiki/display/GDP/Renesas+R-Car+M2+Porter+Hardware+Setup+and+Software+Installation#RenesasR-CarM2PorterHardwareSetupandSoftwareInstallation-GDP-ivi7)

Considerations for specific GDP components
-----------------------------------------------------------

For the Fuel Stop Advisor Proof of Concept (FSA PoC), a navigation map
must be downloaded. Once booted, issue the following command on the board:

# cd /usr/share/navit/maps/ && wget http://www.navit-project.org/switzerland.bin

To build the RVI SOTA client into your GDP image, please check:
[here](https://at.projects.genivi.org/wiki/display/GDP/RVI+SOTA+Client)

For further information about [GDP components](https://at.projects.genivi.org/wiki/pages/viewpage.action?pageId=11567879)

Considerations for specific Peripherals
--------------------------------------------------

Enable touch support on the GENIVI AMM Faytech V2 monitor for Porter & Raspberry Pi 2 add to local.conf:
USE_FAYTECH_MONITOR = "1"

Further information about [GDP and peripherals](https://at.projects.genivi.org/wiki/display/GDP/GDP+and+peripherals)
