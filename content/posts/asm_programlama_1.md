---
author:
  name: Levent Kaya
date: 2022-08-11
linktitle: x86_64 Islemcilerde Assembly Programlama - 1 & 2 - Bilgisayar Mimarisi 
type:
- post
- posts
title: x86_64 Islemcilerde Assembly Programlama - 1 & 2 - Bilgisayar Mimarisi
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

Memorynin disinda CPU icerisinde kendine ozel, hizli hafiza alanlari barindirir. Bu alanlar *register* olarak adlandirilir ve general purpose registers - special purpose registers olmak uzere ikiye ayrilir. General Purpose Registerlarda toplama, cikarma, karsilastirma gibi ana islemler yapilir. Buna ragmen CPUlarda oldukca kisitli sayida register bulunur. Veriler ve talimatlar memoryden CPU'ya tasinir burada islem gordukten sonra data bus vasitasiyla yeniden memorye gonderilir. Serinin ilerleyen kisminda, generap ve special registerlari daha detayli isleyecegiz.

Gereken veriler CPU'ya gonderildikten sonra bu veri ve `decode instructionlar` ALU'ya gelir ve burada gerekli islemi gorur, ardindan sonuc, data bus yardimiyla memoryde ilgili yere geri gonderilir.

## 2.3 Data Accessing Metodlari 
Islemcilerde dataya erismenin birden farkli yolu vardir, bunlara data accesing metodlari denir.

* Bunlardan en basiti `immidiate` moddur. Ornegin bir registeri 0'a esitlemek istiyorsak, bilgisayara 0 degereini okuyacak bir adres vermek yerine direk olarak 0 degerini atariz. 

* `register addressing` mod'da instruction bir memory adresi yerine ulasilacak bir register barindirir. bunlar disinda diger tum modlar adresslerle dataya ulasir.

* `direct` adressing mode'da instruction bir memory adresi barindirir. ornegin bir registera direct mod ile veri yuklemek istersem, bu registera memorydeki xxxx konumuna git derim bunun neticesinde bu registera xxxx konumundaki degeri atamis olurum. Burda onemli olan nokta registera xxxx degerini degil memoryde xxxx adresinde bulunan degeri yukluyor olmam.

* `indexed` modunda, instruction bir hafıza adresi içerir. erişim ve ayrıca bu adresi dengelemek için bir dizin kaydı da belirtir.

* `indirect` adressing modunda, instruction, datanin yerini gosteren pointeri barindiran bir register icerir.

* son olarak `base pointer` adressing mode bulunur, serinin devaminda kullanacagimiz icin simdilik bunu giris kisminda aciklamiyorum.
  
### Bolum 1 Sonu...
Bu bolumde anlatabileceklerim bu kadardi. Assembly programlamak icin bilmemiz gereken temel bilgisayar mimarisi konularini ele almaya calistim. Serinin bir sonraki bolumunden ilk ve dolayisiyla cok basit assembly programimizi yazacagiz.
