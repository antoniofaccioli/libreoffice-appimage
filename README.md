# libreoffice-appimage
This script create the appimage version of LibreOffice.

You can use 5 parameters for the script:
1. The first one indicates what package you want to do. Values can be fresh, still, daily or version number;

2. The second parameter is used to determine if the version of LibreOffice is 32 or 64 bits. The script works for the moment      with the only x86-64 parameter, so only 64 bit;

3. The third serves to indicate the language. Accept the following values:

   a. standard: create a package with some languages (en-GB,it,ar,zh-CN,zh-TW,fr,de,ja,ko,pt,pt-BR,es,ru);
   
   b. full: create a package with all the languages supported by the project (af,am,ar,as,ast,be,bg,bn-IN,bn,bo,br,brx,bs,ca-         valencia,ca,cs,cy,da,de,dgo,dz,el,en-GB,en-ZA,eo,es,et,eu,fa,fi,fr,ga,gd,gl,gu,gug,he,hi,hr,hsb,hu,id,is,it,ja,ka,kk,km,       kmr,kn,ko,kok,ks,lb,lo,lt,lv,mai,mk,ml,mn,mni,mr,my,nb,ne,nl,nn,nr,nso,oc,om,or,pa-IN,pl,pt-BR,pt,qtz,ro,ru,rw,sa-IN,sat,       sd,si,sid,sk,sl,sq,sr-Latn,sr,ss,st,sv,sw-TZ,ta,te,tg,th,tn,tr,ts,tt,ug,uk,uz,ve,vec,vi,xh,zh-CN,zh-TW,zu);
   
   c. one of the language code of the project (eg it);
   
   d. N: no sub language, the package is created with the default language only (en-US).
   
4. The fourth serves to indicate whether you want to enter the offline help. This parameter depends on the third;

5. The fifth parameter specifies whether you want to make a zsync file, which allows you to handle a differential download. For    this option, the zsync tool must be present on the system;

6. The sixth indicates whether you want to sign the created package. You need to install the GPG tool on your system and create    your own signature. If you use this function you need to value the $gpgPass variable with the passphare.

For example, to make a version of the version with the Italian language, you will use the following command:

./libreoffice-appimage still x86-64 it N N N

You will make a LibreOffice still version with the Italian language, without the offline help, will not create a zsync file or even sign the package.
