---
author:
  name: Levent Kaya
date: 2022-08-14
linktitle: Sifirdan Bir Compiler Programlamak - 0 - Giris
type:
- post
- posts
title: Sifirdan Bir Compiler Programlamak - 0 - Giris
weight: 10
series: 
---
# Bir Compiler Programlamak - 0
## Giris

Bir compiler programlamaya karar verdim. Daha onceden basit assemblerlar, disassemblerlar, basit diller icin ufak compilerlar kodlamistim. Ama hic kendi kendini compile edebilen gercek bir compiler programlamadim. 

Bu yuzden bu blog serisinde gercek bir compilerin nasil programlandigini arsivlemek istedim. Blog serisi ve development sureci paralel gercekleseceginden bu yazilari okuduktan sonra yazinin altindaki proje linkinden implement edilmis kodlari da bulabilirsiniz. Umarim benim ve sizler icin yararli olur :)

## TODO
Yazacagim compiler icin kendime birkac hedef belirledim. 
* Self-compiling bir compiler olmali. Yani kendini compile edebilen gercek bir compiler yazmak istiyorum. 
* Gercek bir mimaride calismali. Daha once varsayimsal makinelerde calisan compilerlar gormustum, daha once chip8 gibi basit makine interpreterlari da yazmistim. Bu projenin farkli olmasini ve x86_64 makinelerde gercekten compile islemini gerceklestirebilmesini istiyorum. 
* Burada verecegim bilgiler teorikten ziyade pratik olmali. Compiler developmenti icin efsane dragon booku duymussunuzdir. Yani bu blogu okuyorsaniz duydugunuzu var sayiyorum. Bence harika bir kitap ama bu blog serisinde ana odagim compilerin ne oldugunu anlatmaktan ziyade gercekten calisan bir urun cikarmak. 

## Hedef Dil
Bilgisayar programlari yazmaya yaklasik 10-11 sene once ortaokulda baslamistim. Sicak bir ogleden sonraydi, neden nasil hangi motivasyonla oldu hatirlamiyorum ama youtube tutoriallari izleyip basit bir hesap makinesi yaptigimi hatirliyorum.Bu hesap makinesini C ile yazmistim. Simdi 10 sene sonra bu isi profesyonel olarak icra ederken bunu soylemek ne derece dogru bilmiyorum fakat soyleyecegim, hala, 10 sene dahi sonra en zevk aldigim dil C. Dolayisiyla bu compilerin da hedef aldigi dil C olacak, kendi kendini compile edebilen bir C compileri yazmayi hedefliyorum. Tabiki tum C18 ozelliklerini implement edemem ama elimden geldigince calisan C subseti compile eden bir proje gelistirmeye calisicam. 

## Gelistirme Ortami
Bu blog serisini herhangi bir Linux makinede takip edebilirsiniz. Ben genellikle Arch Linux yada Ubuntu kullaniyorum. 
Arch Linux icin 
```
sudo pacman -S base-devel
```
Debian tabali herhangi bir distro icin de 
```
sudo apt-get install built-essentials
```
Yeterli olacaktir. Siz bu iki distro disinda bir distroda calisiyorsaniz paket managerinize bagli olarak ufak degisiklikler olabilir ama hangi paketi indirmeniz gerektigini bulmak cok zor olmayacaktir. 

## Gelecek Bolumde 
Gelecek bolumde projemizin ilk admini atmaya basliyoruz, basit bir lexer kodlayacagiz bu sayede input olarak gelen c dosyasini tokenlerine ayirabilecegiz. 

Blog serisinin devaminda ve bitiminde ne zaman isterseniz projede yazilmis olan kodlari inceleyebilir, forklayip degistirebilir ve daha iyi olacagini dusundugunuz degisiklikler varsa pull requst acabilirsiniz.

* Proje Linki: [SCC- Simple C Compiler](https://github.com/lvntky/SCC)
