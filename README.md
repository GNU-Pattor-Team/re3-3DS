README below copied from <https://gbatemp.net/threads/wip-re3-3ds-port.586374/>

This repo contains a repack of the porting which inludes all the GTA III game files, you don't need to own the game on PC, just extract the archvive on the 3DS and play.

- [re3-3ds-Repack.tar.xz](https://github.com/GNU-Pattor-Team/re3-3DS/releases/latest)

---

## Intro

This is a WIP port of the re3 project to nintendo 3ds.
Only new 3ds is supported. An old 3ds port might be possible but I'm not going to try.
This release is more suited for developers as I haven't really made anything convenient or easy.

There are four seperate 3dsx files:
re3.3dsx, re3-stereo.3dsx,
re3-txd.3dsx, re3-stereo-txd.3dsx.

bonus files:
miami.3ds miami-stereo.3dsx.

The re3-*-txd builds do texture compression on your 3DS and generate a TXD.IMG / TXD.DIR.
The vanilla builds re3.3dsx, re3-stereo.3dsx assume you have converted the game files to a native format using txdcnv (provided).
The miami.3ds files haven't really been tested in a while and they only do texture conversion on the 3ds (might take literally a day lmao).

There is no CIA version.

GTA3 has reached a certain level of stability that it worth sharing, however consider this an alpha WIP.
Vice City is also working, but has more serious bugs.

## Caveats/Bugs (many)

- Menus and loading screens are blurry (easy fix but its not important).

- Fonts are fucked due to mip-map generation. This is a recent regression from implementing txdcnv.

- Material FX environment mapping isn't accurate. Cars are very shiny. Shinier than the mobile port :DDD.

- Dynamic lighting (directional lights) is broken. I think it has to due with floating point accuracy. This has driven me crazy and I've almost given up as it works with every test program I write.

- Skinned geometry is jagged / glitchy, I suspect it has to do with floating point accuracy. Looks pretty funny though.

- There is a black roads glitch, which I presume has to do with blending.

- MP3 playback slows things down (the audio thread runs on the primary app-core).

- pager/text messaging is unreadable.

- bottom screen is not in use... yet.

- I've only play-tested the first third of the game.

## Install:

### Prerequisites:
- you will need to be able to read instructions.
- you will need the PC version of GTA III. (other versions might work).

### Procedure:
Texture conversion:
You need to use txdcnv to convert the game files to native formats.
Create a directory for the output files.
I have only built txdcnv for Linux x86-64, sorry about that, you will have to use re3-txd.3dsx or compile from source.

./txdcnv [gta3 root] [output root].

The [output root] will now look like:
.:
models txd

./models:
fonts.txd generic.txd gta3.img menu.txd particle.txd
frontend.txd gta3.DIR hud.txd MISC.TXD

./txd:
LOADSC0.TXD LOADSC15.TXD LOADSC20.TXD LOADSC2.TXD LOADSC8.TXD SPLASH2.TXD
LOADSC10.TXD LOADSC16.TXD LOADSC21.TXD LOADSC3.TXD LOADSC9.TXD SPLASH3.TXD
LOADSC11.TXD LOADSC17.TXD LOADSC22.TXD LOADSC4.TXD mainsc1.txd
LOADSC12.TXD LOADSC18.TXD LOADSC23.TXD LOADSC5.TXD mainsc2.txd
LOADSC13.TXD LOADSC19.TXD LOADSC24.TXD LOADSC6.TXD NEWS.TXD
LOADSC14.TXD LOADSC1.TXD LOADSC25.TXD LOADSC7.TXD SPLASH1.TXD

Mipmaps are actually 100% necessary.
They drastically improve performance and memory management.
Older versions did this on the 3ds but it took too long.

### Updating game directory

You will overwrite the files in [Game root] with these modified files.
Copy the files from [Output root] to [Game root].
From (this archive) src/gamefiles copy "data" and "TEXT" to the [Output root].
This will overwrite PARTICLE.CFG and various GXT files.

### 3ds File system
The game files are stored in a hard-coded directory on your 3ds.
Create the directory 'sdmc:/3ds/re3'. (or sdmc:/3ds/miami for VC)

You can copy the contents of [Game root] to /3ds/re3,
but if your a bit more OCD you only need the following directories:

sdmc:/3ds/re3/anim
sdmc:/3ds/re3/audio
sdmc:/3ds/re3/data
sdmc:/3ds/re3/models
sdmc:/3ds/re3/mp3
sdmc:/3ds/re3/skins
sdmc:/3ds/re3/txd

### Music files
Copy .wav|.mp3 to sdmc:/3ds/re3/mp3.
Sorry .ogg isn't supported, but would be very easy to enable.

### Install 3dsx

Copy re3.3dsx and re3-stereo.3dsx to sdmc:/3ds/ (or anywhere really).
Launch with Homebrew launcher.

To access the Free-Roam script hold [L, R, dpad-up] in the main menu.
To access the Debug script hold [L, R, dpad-down] in the main menu.
To access the debug menu in-game hold [L, R, dpad-down], then use the touch screen.

### Building:
You only need devkitpro / devkitARM.
The Makefile has only been tested on linux (devuan beowulf), but it should work on mingw/msys or cygwin.
I've included all the libraries because devkitpro is a rolling release and I can't
assume that future versions of the libraries won't break the build. Its a pain to manually
check-out each specific version of each library. I have enough trouble with my own regressions.

Make sure you environment has DEVKITPRO / DEVKITARM.
$ . /etc/profile.d/devkit-env.sh
$ cd re3/build
$ make

or for stereo set STEREO in your env.

$ STEREO=1 make

make upload sends to hb-menu.
make debug call gdb and connects to 10.0.0.2.

### Misc notes:
I'm a bit ashamed by my misuse of git.
I haven't committed anything yet and all the new stuff is untracked.
A good starting point for customization is in src/core/config.h.
If you want to enable online texture compression enable TXD_USE_CDIMAGE.
There are two versions of librw in vendor for mono and stereo rendering respectively.
3ds platform specific code is at src/skel/3ds.
I can't make any promises on updates, I had to force myself to make this release and I'm getting burnt out and need a break.
You should probably build from source, I haven't got the time to test the binaries and I keep making big regressions every time I improve something.

## LICENSE? REDISTRIBUTING?
I dunno, the re3 team seems to re3 as public domain(?), but the libraries are GPL.
Feel free to share wherever but I request that if you distribute the source code as well.
At the very least please provide librw's source code, nobody will sue you for that.

## DOWNLOAD
- 3DSX: https://mega.nz/file/bd0gyAJZ#3z92A6nL9ZFrjsBfIC7KRmLgL7F0_eKv50SqPNVmfAg
- SOURCE: https://mega.nz/file/bF1UUSoL#S6Eby9ooGC4R6RHCD8zubykULZARnPl0rTyCplrbH-c
