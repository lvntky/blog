---
author:
  name: Levent Kaya"
date: 2022-04-05
linktitle: x86_64 Islemcilerde Assembly Programlama - 1
type:
- post
- posts
title: x86_64 Islemcilerde Assembly Programlama - 1 
weight: 10
series:
---
# BOLUM 1 - BASLANGIC

Merhabalar, bu blog serisinde x86_64 mimarisine sahip islemcilerde Assembly programlamayi anlatiyor olacagim. 

Bu seride Jonathan Bartlett'in Programming From Ground Up kitabini referans alacagim.

## 1.1 Gelistirme Ortami

Bu blog serisine baslarken gelistirme ortamindan bahsetmek istiyorum. Seri boyunca programlari GNU\Linux ortaminda gelistirecek ve GCC tool setini kullanacagim. Bu seriyi bir Linux makinada takip etmenizi siddetle oneririm. Ben Arch Linux kullaniyorum fakat kullandiginiz distronun pek de bir onemi yok. 

# BOLUM 2 - BILGISAYAR MIMARISI

Bir program yazmaya baslamadan once, bilgisayarlarin yazdigimiz programlari nasil anlamlandirdigini anlamamiz gerekiyor. Assembly ogrenmenin en iyi yani da bu, neyi neden yaptigimizi ve tum bunlarin nasil gerceklestigini anlamya yetenegi kazaniyoruz.

Modern bilgisayarlar Von Neumann mimarisini baz alarak calisirlar. Von Neumann mimarisi bilgisayarlari CPU ve Memory olmak uzere iki ana parcaya ayirir.

## 2.1 Memory

Bilgisayar memorysi bir posta kutusuna cok benzer. Her ikisinin de belirli sayida mektup yada veri tutma kapasitesi vardir. Ornegin 256kBlik bir bilgisayar memorysine sahipseniz kabaca, 256 milyon belirli sayida veri tutma alanina sahipsiniz demektir. Her alanin bir adresi ve ayni sabit sayida veri tutma alani vardir. 


![Memory](/https://github.com/lvntky/blog/blob/main/static/images/memory.png)

Bilgisayarimizla ilgili kaydedilmesi gereken hemen her sey memory'de bulunur. 

* ekrandaki imleciniz konum
* su an bu blogu okudugunuz browserin ebati
* kullandiginiz yazi fontunun buyuklugu
* toolbarinizdaki grafikler
* ve akliniza gelebilecek diger tum seyler!

Buna ek olarak, Von Neumann mimarisine gore bilgisayarimizi kontrol eden programlar da burada, memoryde saklanir. Bilgisayarimizin gozunden bir kaynak kodu yani yazdigimiz program ve bu programin ciktisi arasinda hicbir fark yokturk.

## 2.2 CPU

Peki bir bilgisayar nasil isler? Tahmin edersiniz ki verileri tutmak bir bilgisayarin islemesi icin yeterli degildir. Calisan bir bilgisayar icin bu verilere ulasmamiz, degistirmemiz, uzerinde islemler yapabilmemiz de gerekir. Bu noktada bu gorevi CPU ustlenir. 

CPU memorydeki verileri okur ve calistirir. Bu isleme *fetch-execute cycle* denir. Bu gorevi tamamlamak icin bir CPU asagidaki bilesenlere ihtiyac duyar:

* Program Counter
* Instruction Decoder
* Data Bus
* General Purpose Registers
* ALU (Arithmatic Logic Unit)

*Program Counter* memoryden gelecek bir sonraki talimatin yerini gosterir. Boylece fecth execute cycle aksamadan devam edebilir. Herhangi bir veri ile bir pgoramin kaynak kodunun memoryde ayni sekilde tutuldugunu sadece CPU nun bu iki farkli veriyi isleme biciminin farkli oldugunu soylemistik. Program Counter islenecek bir sonraki talimatin adresini tutar, CPU Program Counter'a bakar ve tuttugu memory adresinde hangi talimat varsa onu calistirir.

Ardindan memoryden alinan bu talimat, *instruction decoder* a gelir. Burada bu talimatin CPU'dan ne gibi bir islem bekledigi ve bu islemden hangi verilerin etkilenecegi anlamdirilir(ornegin memorydeki 1. sayi ile 2.sayiyi topla)

Bu atama islemleri icin *Data Bus* kullanilir. Data Bus memory ve CPU arasindaki koprudur, yapilmasi istenilen talimatta isleme hangi memory adreslerinin dahil olup olmayacagi bu kopru ile saglanir. 

