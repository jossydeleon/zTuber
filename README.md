# FFmpeg for Android

This tutorial will show how to compile ffmpeg 3.0 to use the powerful command line tool in any Android application

# Enviroment:
* Ubuntu 16.04 LTS
* Memory 3.8 GiB
* Intel® Core™ i5-3320M CPU @ 2.60GHz × 4

# Software required:
* FFmpeg 3.0
* Android NDK
* Android Studio


1. Open a terminal and download Android NDK, then unzip
* `wget http://dl.google.com/android/repository/android-ndk-r12b-linux-x86_64.zip`
* `unzip android-ndk-r12b-linux-x86_64.zip` 

or download the latest android ndk from here: https://developer.android.com/ndk/downloads/index.html and unzip



2. Inside NDK folder go to **sources** and create a new folder called **external-libs** **(if you want to compile any other lib with ffmpeg like libmp3lame, libx264, etc)**



3. Download FFmpeg 3.0 from https://www.ffmpeg.org/releases/ and unzip inside 
**sources** folder. You will have ffmpeg-3.0 folder



4. Update the file **configure** inside Ffmpeg folder

**Replace this:**

`SLIBNAME_WITH_MAJOR='$(SLIBNAME).$(LIBMAJOR)'
LIB_INSTALL_EXTRA_CMD='$$(RANLIB) "$(LIBDIR)/$(LIBNAME)"'
SLIB_INSTALL_NAME='$(SLIBNAME_WITH_VERSION)'
SLIB_INSTALL_LINKS='$(SLIBNAME_WITH_MAJOR) $(SLIBNAME)'`

**For this:**

`SLIBNAME_WITH_MAJOR='$(SLIBPREF)$(FULLNAME)-$(LIBMAJOR)$(SLIBSUF)'  
LIB_INSTALL_EXTRA_CMD='$$(RANLIB)"$(LIBDIR)/$(LIBNAME)"'  
SLIB_INSTALL_NAME='$(SLIBNAME_WITH_MAJOR)'  
SLIB_INSTALL_LINKS='$(SLIBNAME)`



5. Create temporal folder named **ffmpegtemp** inside FFmpeg folder.



6. Create new document called **ffmpeg_build.sh** inside FFmpeg folder and copy this:
**Note: You can edit this file with the options that you want to setup for ffmpeg**

**edit the NDK variable with your NDK path**

**Copy this:**
```
#!/bin/bash
export TMPDIR=/your/path/ffmpeg/ffmpegtemp
NDK=/your/android/ndk/path/android-ndk
PLATFORM=$NDK/platforms/android-14/arch-arm
TOOLCHAIN=$NDK/toolchains/arm-linux-androideabi-4.9/prebuilt/linux-x86_64

echo ""
echo ""
echo "********************* Zoe Developers by Josymar ***********************"
echo ""
echo "Compiling Shared library FFmpeg 3.0 for Android"
echo ""
echo "****************************************************************" `

function build_one
{
./configure \
    --prefix=$PREFIX \
    --target-os=linux \
    --cross-prefix=$TOOLCHAIN/bin/arm-linux-androideabi- \
    --arch=arm \
    --sysroot=$PLATFORM \
    --extra-cflags="-I$NDK/sources/external-libs" \
    --extra-ldflags="-L$NDK/sources/external-libs" \
    --cc=$TOOLCHAIN/bin/arm-linux-androideabi-gcc \
    --nm=$TOOLCHAIN/bin/arm-linux-androideabi-nm \
    --disable-static \
    --enable-shared \
    --disable-runtime-cpudetect \
    --enable-gpl \
    --enable-small \
    --enable-cross-compile \
    --enable-asm \
    --enable-neon \
    --enable-yasm \
    --disable-debug \
    --disable-doc \
    --disable-ffmpeg \
    --disable-ffplay \
    --disable-ffprobe \
    --disable-ffserver \
    --disable-postproc \
    --disable-avdevice \
    --disable-symver \
    --disable-stripping \
    --extra-cflags="-Os -fpic $ADDI_CFLAGS" \
    --extra-ldflags="$ADDI_LDFLAGS" \
$ADDITIONAL_CONFIGURE_FLAG
make clean
make -j$(nproc)
make install

}

#for arm 
CPU=arm
PREFIX=$(pwd)/android/$CPU 
ADDI_CFLAGS="-marm"
build_one
```


7. Open a terminal and give permissions to the file created 
* `chmod +x ffmpeg_build.sh`



8. Now compile the FFmpeg 
* `bash ffmpeg_build.sh`
**Note: This process will take some time, depending your machine.**



9. After this, you will see a new folder called **android**. Your compile files will be inside the folder **arm/lib**. Just keep the files with a dash and number. For example **libavcodec-57.so**. You can deleted the original versions **libavcodec.so**



### Please give me some time to show how to use this files into an android app.


