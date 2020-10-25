# `make_libreoffice_appimage`
## (Re-package official LibreOffice .deb packages into the AppImage format)

This script creates an AppImage package for any version of LibreOffice.

As its input files it fetches the official Debian binary (*`.deb`*) packages built and archived by *[The Document Foundation](https://www.documentfoundation.org/)* itself (i.e. the developers and publishers of LibreOffice).

It then just re-packages these archives and adds the magic *`AppRun`* from *[AppImage](https://github.com/AppImage)* into it. The result will be ONE executable file containing ALL of the LibreOffice suite. This file you can directly run on (almost) any x86-64 based Linux system you can think of. It will not add any libraries to the system, and it will not mess with the packages installed by your native package manager (zypper, apt, yum, rpm, whatever...). If you want to get rid of this (we bet: you won't), all you have to do is to delete that one file, appropriately called *LibreOffice-<versionstring>-x86-64.AppImage*. It will leave no trace on your system (other than the documents you may have created with its help).

This script can re-package the complate range of any old version of LibreOffice (if still available thru the download archives of The Document Foundation) up to the latest and hottest "daily" builds published thru the 6.0.0.0.alpha channel.

The best thing is, you can package different versions of LibreOffice into an AppImage file, and you can run them side-by-side on the same system, without interfering with each other. This is also a great tool to help triaging bugs for the LibreOffice community.

## Usage

The script takes 6 parameters:

1. **First:**  indicate which package *"channel"* you want to use as input.
   Values can be 'fresh', 'still', 'daily' or the specific (existing) version number (like '5.4.2.2').

2. **Second:** determine if the requested version of LibreOffice is 32 or 64 bits.
   This parameter can be 'x86' (for 32-bit) or 'x86-64' (for 64-bit).

3. **Third:** request the language pack(s) you want to use with LibreOffice. The following values may be submitted:
   - `'standard'`: creates a package with a set of pre-defined languages, namely: *nl, en-GB, fr, de*;
   - `'full'`:  creates a package with ALL the languages supported by the project, namely: *af, am, ar, as, ast, be, bg, bn-IN, bn, bo, br, brx, bs, ca-valencia, ca, cs, cy, da, de, dgo, dz, el, en-GB, en-ZA, eo, es, et, eu, fa, fi, fr, ga, gd, gl, gu, gug, he, hi, hr, hsb, hu, id, is, it, ja, ka, kk, km, kmr, kn, ko, kok, ks, lb, lo, lt, lv, mai, mk, ml, mn, mni, mr, my, nb, ne, nl, nn, nr, nso, oc, om, or, pa-IN, pl, pt-BR, pt, qtz, ro, ru, rw, sa-IN, sat, sd, si, sid, sk, sl, sq, sr-Latn, sr, ss, st, sv, sw-TZ, ta, te, tg, th, tn, tr, ts, tt, ug, uk, uz, ve, vec, vi, xh, zh-CN, zh-TW, zu*;
   - `'<custom>'`:  one or more comma separated language codes out of those supported by the project (for full list see above), f.e. `it` or `de,es`;
   - `'N'`:  no extra language at all; the package will contain support for the default language only (which is `en-US`).

4. **Fourth:** request to package (or not) offline help into the bundle.:
   - `'Y'`:  yes, embed the offline help in the AppImage for the same languages as selected for the third parameter above;
   - `<custom>`:  one or more comma separated language codes out of those supported by the project (for full list see above), f.e. `it` or `de,es`;
   - `'N'`:  no, do NOT embed the offline help in the AppImage.

5. **Fifth:**  specify if you want (or not) to be able to make the package updateable. (This additionally creates a *zsync* file, which is used to handle "differential" downloads, so an update does not have to fetch a *complete* AppImage file, but only those byte-ranges which have changed). For this option to work, the [zsync-curl](https://github.com/AppImage/zsync-curl) tool must be present on the system. Possible values are:
   - `'Y'`:  yes, make the AppImage updateable;
   - `'N'`:  no, do NOT make the AppImage updateable.

6. **Sixth:** indicate whether you want to sign the created package. You need to install the GPG tool on your system and create your own signature. **If you use this function you need to put a fill the (by default empty) `$gpgPass` variable inside the script with the passphrase as its value.** Possible values are:
   - `'Y'`:  yes, sign the AppImage;
   - `'N'`:  no, do NOT sign the AppImage.

7. **Seventh:** indicate whether you want to use a specific profile folder for the package or use the default LibreOffice configuration.
The possible parameters are:
   - `'Y'`:  yes, use a specific profile folder;
   - `'N'`:  no, LibreOffice default configuration is maintained.


## Examples

**Example 1:**  **`../make_libreoffice_appimage still x86-64 it N N N N`**
             Build LO AppImage package with Italian language-pack, without offline help, not auto-updateable, without signing the package, not portable.

**Example 2:**  **`../make_libreoffice_appimage daily x86-64 de Y Y N Y`**
             Build LO AppImage package (development version) with German language-pack, with (German) offline help, updateable via zsync, without signing the package, portable.

**Example 3:**  **`../make_libreoffice_appimage 3.5.7.2 x86-64 standard Y N N N`**
             Build LO v3.5.7.2 AppImage package with a set of selected language-packs, with offline help (for selected languages), not auto-updateable, without signing the package, not portable.

## FAQ

**Can I download appimage packages for Libreoffice?**

Yes, you can find a dedicated page on the official website https://www.libreoffice.org/download/appimage/ with the basic packages otherwise you can go download other versions via https://libreoffice.soluzioniopen.com/

**Can I trust this script?**

Check it out. The code is here. If you have a question, ask on IRC in channels [#AppImage](https://webchat.freenode.net/?channels=appimage) or [#libreoffice-appimage](https://webchat.freenode.net/?channels=libreoffice-appimage).

**Can I trust the AppImage, when it runs?**

Do you trust the LibreOffice code you run, if it is installed from RPM or DEB packages? Are you aware, that their respective *pre/postinst* scripts are run by your distro's package manager with *root* privileges? AppImage does not use any root privileges at all!

* If you trust LibreOffice RPMs or DEBs, you implicitely trust the developers of LibreOffice as well as these packagers already to a very high degree.

* The AppImage, once created, contains the very same executable binaries as the respective LibreOffice version which was released by The Document Foundation as a DEB package.

* The DEB packages which served as input to the AppImage packaging toolchain was created by maintainers who are part of the LibreOffice project themselves.

* Yes, a thin layer of additional code that is executed when running the AppImage, the *[AppRun](https://github.com/AppImage/AppImageSpec/blob/master/draft.md#the-apprun-file)*, was created and is maintained by the developers of the [AppImage tools](https://github.com/AppImage). It is also OpenSource. Check it out. Or ask a friend whose technical abilities you trust to check it out.

* One thing you can do, while the AppImage runs:
  In a terminal, run

      mount | grep \.mount_Lib

  This will reveal the (temporary) mount point where the AppImage filesystem is (read-only) mounted while LibreOffice runs. You can move into that directory freely and look around and even copy files to someplace outside the AppImage. Then you can keep investigating them when the AppImage does not run any more, once you closed down LibreOffice....

**Can I create different versions of LibreOffice AppImages and store them on the same system?**

Yes.

**Can I RUN different versions of LibreOffice AppImages at the same time on the same system?**

Yes.

**How do I get rid of the AppImage again, should I no longer want it?**

Just delete the one AppImage file.

**Where do I have to store the AppImages?**

Store them anywhere you want.

If you like a neat environment, we'd recommend to create a directory such as *`$HOME/AppImages`* and store them all there.

**It's awkward to run an AppImage from the command line with such a long name!**

Is that a question? We agree.

Maybe your distro already created a *`$HOME/bin`* subdirectory and put it in your *`$PATH`*? If not, you can do so yourself.

Then just create a symlink in there by the name of *loffice* (or whatever you like) which points to the exact location of the AppDir: `cd $HOME/bin ;  ln -sf ../AppImages/LibreOffice-<versionstring>-x86_64.AppImage  loffice`.

Now you can type *loffice* to run the AppImage.

**The symlinking stuff is nice, but I want it NICER!**

Check out (the completely OPTIONAL) [AppImageKit](https://github.com/AppImage/AppImageKit).

It contains a few small extra tools to handle AppImages (also to look what's inside, or mount them without running the AppRun).

One of them is *`appimaged`*, a little daemon which automagically discovers new AppImages and creates entries in Desktop menues so you can "click" around your ways. (*`appimaged`* also automagically removes desktop entries for AppImages which you delete.)
