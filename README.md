# `make-libreoffice-appimage`
## (Re-package official LibreOffice binaries into the AppImage format)

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
   At this time this parameter MUST be 'x86-64'(so only 64 bit), because AppImage does not (yet) support 32 bit.
   (So this parameter is there to support the possible future use of 32bit.)

3. **Third:** request the language pack(s) you want to use with LibreOffice. The following values may be submitted:
   - `'standard'`: creates a package with a set of pre-defined languages, namely: *en-GB, it, ar, zh-CN, zh-TW, fr, de, ja, ko, pt, pt-BR, es, ru*;
   - `'full'`:  creates a package with ALL the languages supported by the project, namely: *af, am, ar, as, ast, be, bg, bn-IN, bn, bo, br, brx, bs, ca-valencia, ca, cs, cy, da, de, dgo, dz, el, en-GB, en-ZA, eo, es, et, eu, fa, fi, fr, ga, gd, gl, gu, gug, he, hi, hr, hsb, hu, id, is, it, ja, ka, kk, km, kmr, kn, ko, kok, ks, lb, lo, lt, lv, mai, mk, ml, mn, mni, mr, my, nb, ne, nl, nn, nr, nso, oc, om, or, pa-IN, pl, pt-BR, pt, qtz, ro, ru, rw, sa-IN, sat, sd, si, sid, sk, sl, sq, sr-Latn, sr, ss, st, sv, sw-TZ, ta, te, tg, th, tn, tr, ts, tt, ug, uk, uz, ve, vec, vi, xh, zh-CN, zh-TW, zu*;
   - `'<single>'`:  one single extra language code out of those supported by the project (for full list see above), f.e. `it`;
   - `'N'`:  no extra language at all; the package will contain support for the default language only (which is `en-US`).
   
4. **Fourth:** request to package (or not) the offline help into the bundle. This the offline help language will be the same as selected for the third parameter above:
   - `'Y'`:  yes, embed the offline help;
   - `'N'`:  no, do NOT embed the offline help.

5. **Fifth:**  specify if you want (or not) to be able to make the package updateable. (This additionally creates a *zsync* file, which is used to handle "differential" downloads, so an update does not have to fetch a *complete* AppImage file, but only those byte-ranges which have changed). For this option to work, the [zsync-curl](https://github.com/AppImage/zsync-curl) tool must be present on the system.

6. **Sixth:** indicate whether you want to sign the created package. You need to install the GPG tool on your system and create your own signature. If you use this function you need to put a fill the (by default empty) `$gpgPass` variable inside the script with the passphrase as its value.


## Examples

**Example 1:**  **`../make_libreoffice_appimage still x86-64 it N N N`**
             Build LO AppImage package with Italian language-pack, without offline help, not auto-updateable, without signing the package.

**Example 2:**  **`../make_libreoffice_appimage daily x86-64 de Y Y N`**
             Build LO AppImage package (development version) with German language-pack, with (German) offline help, updateable via zsync, without signing the package.

**Example 3:**  **`../make_libreoffice_appimage 3.5.7.2 x86-64 es Y N N`**
             Build LO v3.5.7.2 AppImage package with Spanish language-pack, with (Spanish) offline help, not auto-updateable, without signing the package.
