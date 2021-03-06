Make Testing Toybox not a 5 Hour Affair
=======================================

This repo contains build system extensions to create a minimal Android
Target "image" which can be run using a standard qemu system


Getting Started - Testing Bionic Toybox when the HOST_ARCH == TARGET_ARCH
=========================================================================


repo init --depth 1 -u https://github.com/trevd/android_vendor_toybox
repo sync --current-branch

source build/envsetup.sh

lunch aosp_x86_64-eng

make -j16 toybox-test

sudo ln -s $ANDROID_PRODUCT_OUT/system /system 

/system/bin/toybox 


Contents
========

                    
                    

./build/core/definitions.mk
Override standard build system variables to use our prebuilt versions
of the required host tools and switch off host side compiling

./build/tasks/toybox.mk
Add the toybox-test target recipe.

./rootdir/Android.mk
Make file fragment for our custom init.rc

./root/init.rc
The aforemention custom init.rc.

./tools
As we turn off host tool compilation we need include prebuilt
versions of the few host tools we require.

./tools/acp
Prebuilt version of android copy ( cp ). Source code and reason for this
tool existing at all can be found in tools/acp of the main build system
directory

./tools/mkbootfs
Statically linked version of Android's mkbootfs utility. This is a glorified
implimentation of cpio -i which automatically handles setting the correct
permissions and selinux contexts for files in the ramdisk
