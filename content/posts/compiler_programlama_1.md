---
author:
  name: Levent Kaya
date: 2022-10-14
linktitle: Sifirdan Bir Compiler Programlamak - 1 - Lexer
type:
- post
- posts
title: Sifirdan Bir Compiler Programlamak - 1 - Lexer
weight: 10
series: 
---

# Bir Compiler Programlama - 1
## Lexer

Serinin bu yazisinda compilerimizi gercekten programlamaya basliyoruz. Ilk implement etmemiz gereken yapi bir lexer.

Lexerin amaci gelen input kayank kodu lexical elementlerine yani tokenlerine ayirmak. Bugun sadece 5 tane token implement edecegiz ama merak etmeyin, seri sonunda sikintisiz calisan bir C compilerimiz olucak. 

Bugun lexerimizin tanimasini istedigimiz tokenler ```+``` , ```-```, ```*``` , ```/``` ve ```INTEGER_LITERAL``` tokenleri. 

### DEFS.H
Tokenleri tutmak icin bir yapiya ihtiyacimiz var, bunun icin defs(definitions).h adinda bir header dosyasi olusturacak ve tokenlerimizi burda tutacagim.

```
struct token
{
	int token;
	int int_value;
};
```
Gordugunuz gibi tokenimizi ve eger tokenimiz bir INTEGER_LITERAL ise onun degerini bu struct icinde tutacagiz. Ayrica bir Enum icinde tokenlerimizin neler oldugunu da belirtmeliyiz ki bu tokenlerle karsilastigimizda onlarin ne oldugunu anladiktan sonra output olarak da soyleyebilelim. 

```
enum 
{
  TOKEN_PLUS, TOKEN_MINUS, TOKEN_STAR, TOKEN_SLASH, TOKEN_INTLIT
};
```

### DATA.H
Bu header dosyasinda tokenlerden ziyade uzerinde lexial analiz yaptigimiz kaynak kodun verilerini tutuyorum. 

```
extern_ int line;
extern_ int putback_token;
extern_ FILE *infile;
```

```line``` degiskeni olasi bir hatada (ornegin tanimlanamayan bir token) hatanin hangi satirda gerceklestigini kullaniciya bildirmek icin tutuluyor.
```putback_token``` bazi durumlarda bu lexical analizi yaparken gerekenden fazla karakteri okuyabiliriz, bununun icin fazla okunan tokeni bu degiskende tutup streame geri gondermek icin bu degiskeni kullaniyoruz.
```infile``` degiskeni direk bizim uzerinde analiz yaptigimiz ```*.c``` kaynak kodumuz, yani simdilik txt dosyalari uzerinde test yapiyoruz ama umarim ileride .c olacak :)

### SCAN.C
Lexerimizda asil isi yapan dosyamiz bu. Ilk once lexical analiz icin bir sonraki karaktere gecme fonksiyonunu kodladim. 

```
static int next()
{
	int c;
	if(putback_token) {
		c = putback_token;
		putback_token = 0;
		return c;
	}
	c = fgetc(infile);
	if(c == '\n'){
		line++;
	}
	return c;
}
```
Eger streamde dolasirken putback_tokenimiz 0 dan farkli bir deger almissa bu fazla ileri gittigimiz ve streame o tokeni geri koymamiz gerektigi anlamina geliyor. Bu yuzden c degiskenimizi putback_token degiskenimize atiyor, put_backi yeniden sifirliyor ve c yi donduruyoruz. Eger boyle bir durum yoksa fgetc ile streamden yeni karakterimizi aliyoruz.

Eger yeni bir satira gecmissek bu sefer de line degiskenimizi arttiyoruz ki hangi satirda oldugumuzu bilelim ve sikinti cikmasi halinde bunu kullaniciya bildirebilelim.

```
static int skip() 
{
	int c; 
	c = next();
	while(c == ' ' || c == '\t' || c == '\n' ||  c == '/r' || c == '\f') {
		c = next();
	}
	return c;
}
```
Bu fonksiyonda mevcut karakterimizin bir token degil de whitespace ile karsilastiginda skiplemesini sagliyoruz.

```
// scan the tokens
int scan(struct token *t) 
{
	int c;

	c = skip();

	switch(c) {
		case EOF:
			return 0;
		case '+':
			t->token = TOKEN_PLUS;
			break;
		case '-':
			t->token = TOKEN_MINUS;
			break;
		case '*':
			t->token = TOKEN_STAR;
			break;
		case '/':
			t->token = TOKEN_SLASH;
			break;
		default:
		  if(isdigit(c)){
		    t->token = TOKEN_INTLIT;
		    t->int_value = scanint(c);
		  }
		  
	}
	return 1;
}
```

asil scan islemi burada gerceklesiyor. Karakterimizin karsilastigi tokene gore token tipini set ediyoruz. Eger bir integer ile karsilasmissak tokenin bir integer oldugunu ve degerini set ediyoruz. 

### MAIN.C
Geriye bir tek lexerimizi calistirmak kaliyor. 
```
static void init() 
{
	line = 1;
	putback_token = '\n';
}
```
Init fonksiyonu ```data.h``` headirindaki dosya bilgilerini set ediyor.

```
char *tok_str[] = {"+", "-", "*", "/", "intlit"};

static void scan_file() 
{
	struct token t; 
	while(scan(&t)) {
		printf("token %s\n", tok_str[t.token]);	
	}
	if(t.int_value == TOKEN_INTLIT) {
		printf(", value %d", t.int_value);
	}
	printf("\n");
}
```
Burada dosyamizi scan etmeye basliyoruz. tabiki ```scan.c``` deki scan() fonksiyonumuz yardimi ile. Ardindan karsilastigimiz ilgili tokenleri output olarak yazdiriyoruz.

```
void main(int argc, char** argv) 
{
	if(argc != 2) {
		usage(argv[0]);
	}
	init();
	
	if((infile = fopen(argv[1], "r")) == NULL) {
	  fprintf(stderr, "Unable to open %s: %s:\n", argv[1], strerror(errno));
		exit(1);
	}
	scan_file();
	exit(0);
}
```
main() fonksiyonu ise tum bunlari calistiriyor test icin hazirladigimiz dosyalari okuyor ve scan ediyor. 

## Test ve Ciktilar
tokenleri almak bu kadardi, simdi bu 5 token icin ufak tefek bir kac test dosyasi hazirladim, sonuclara bakalim.

[x] TEST 01
test01 dosyamizin icerigi su sekilde 
```
3 + 5 / 2 - 3 * 8
```
Bizim scannerimizin ```$ ./scanner test/test01``` cikitisi ise soyle:
```
token intlit
token +
token intlit
token /
token intlit
token -
token intlit
token *
token intlit
```
ilk test icin epey iyi calisti. Diger teslerin icerik ve ciktilari da su sekilde 

[x] TEST02
test02 dosyasi icerigi 
```
2 + 3 * 5 - 8 / 3
```
program ciktisi
```
token intlit
token +
token intlit
token *
token intlit
token -
token intlit
token /
token intlit
```

[x] TEST03
test03 dosyasi icerigi
```1```
program ciktisi
```
token intlit
```

Evet farkli durumlar icin lexerimizin oldukca iyi calistigini gorduk, bu epey mutluluk verici. 

## Bir Sonraki Bolumde
tokenleri taniyan, fazla scan ettiginde bunlari geri veren, whitespaceleri gorunce skipleyen bir lexer yazdik. Serinin bir sonraki bolumunde bir parser yazicak ve dilimizin gramerini cozumlemeye calisacagiz

* Unutmadan tekrar hatirlatayim, bu blog serisinde yazdigim her satir kodu paylasmam imkansiz. Eger kodun tamamini gormek, degistirmek, kullanmak yada katkida bulunmak istiyorsaniz, her seri sonunda projenin blog linkini paylasiyorum.

[SCC- Simple C Compiler](https://github.com/lvntky/SCC)
