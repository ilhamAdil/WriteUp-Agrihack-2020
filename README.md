# WriteUp-Agrihack-2020

# Binary Exploitation
## FLOW 1 (100 Pts)

Diberikan sebuah source code:
```
#include <stdlib.h>
#include <unistd.h>
#include <stdio.h>

//gcc flow1.c -m32 -fno-stack-protector -no-pie -o flow1

void logo(){
  puts("██╗     ██╗███╗   ██╗███████╗");
  puts("██║     ██║████╗  ██║╚══███╔╝");
  puts("██║     ██║██╔██╗ ██║  ███╔╝ ");
  puts("██║     ██║██║╚██╗██║ ███╔╝  ");
  puts("███████╗██║██║ ╚████║███████╗");
  puts("╚══════╝╚═╝╚═╝  ╚═══╝╚══════╝");
}

void setup(){
	setvbuf(stdin,0,2,0);
	setvbuf(stdout,0,2,0);
}


int main(int argc, char **argv)
{
  logo();
  setup();
  volatile int modified;
  char buffer[64];
  modified = 0;
  
  printf("Put some text: ");

  gets(buffer);

  if(modified != 0) {
      system("cat flag.txt");
  } else {
      printf("Try again?\n");
  }
}
```

Dan sebuah nc 52.187.65.2 17001.
Dari code kita tahu bahwa buffer overflow terjadi ketika data char lebih dari 64 byte, langsung saja kita input karakter lebih dari 64 byte

```
ilham@ilham-Acer:~$ nc 52.187.65.2 17001
██╗     ██╗███╗   ██╗███████╗
██║     ██║████╗  ██║╚══███╔╝
██║     ██║██╔██╗ ██║  ███╔╝ 
██║     ██║██║╚██╗██║ ███╔╝  
███████╗██║██║ ╚████║███████╗
╚══════╝╚═╝╚═╝  ╚═══╝╚══════╝
Put some text: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
agrihack{Toooooooooo_much_character_is_dangerous_LINZ_IS_HERE}
/home/ctf/run_challenge.sh: line 2:   853 Segmentation fault      (core dumped) ./chall
```

**flag: agrihack{Toooooooooo_much_character_is_dangerous_LINZ_IS_HERE}**



# Cryptography

## Basic64 (100 pts)
Diberikan file enc.txt dan dilihat isinya:

![image22](https://user-images.githubusercontent.com/66354919/140281216-b8237a0a-cfcb-4de1-8c99-9808ea6cd34b.png)

Karena hintnya adalah base64, langsung saja kita decode base64 tersebut
```
In [4]: import base64

In [5]: base64.b64decode("YWdyaWhhY2t7YmFzaWNfdG9fa25vd19iYXNlNjR9")
Out[5]: 'agrihack{basic_to_know_base64}'
```
**sehingga flagnya: agrihack{basic_to_know_base64}**

## Introduction to Cipher_Series : ROMAN (120 pts)
Diberikan file caesar.png:

![image60](https://user-images.githubusercontent.com/66354919/140281378-d4b26a0b-1721-4c08-bfa6-8ab725a9da89.png)

Dilihat dari nama filenya, Isinya ciphertext yang kemungkinan caesarian cipher. Kita coba decode caesarian cipher tersebut di rumkin.com dengan percobaan shift dari 1 sampai 26.

![image38](https://user-images.githubusercontent.com/66354919/140281866-15332576-b255-417e-8cbf-0169d2a2a1fb.png)

**Dapet flagnya: agrihack{Gaius_Julius_Caesar}**

## Introduction to Cipher_Series : GiovanBattistaBellaso (120 pts)

Diberikan file enc.txt dan gambar seorang tokoh yang megang kunci dengan tambahan kata vibonacci. Hmmm… sepertinya kunci tersebut digunakan untuk mendecode ciphertext tersebut.

![image30](https://user-images.githubusercontent.com/66354919/140281985-95497aaa-210d-4969-81e0-16512754c421.png)

Setelah saya searching apa itu Giovan Battista Bellaso di sepuh google, ternyata tokoh tersebut yang menciptakan sandi vinegere. Langsung saja kita decode vinegere cipher di rumkin.com dengan key vibonacci.

![image6](https://user-images.githubusercontent.com/66354919/140282076-4c740b5e-79a7-46ae-94d6-f08b3377c6b9.png)

**Dapetlah flagnya: agrihack{Vignere_Ch1p3r’s_H3r3}**

## Introduction to Cipher_Series : Talking (120 pts)

![image36](https://user-images.githubusercontent.com/66354919/140282225-a8d47b50-768d-4e3d-a7af-1153211691bb.png)

Diberikan file yg isinya angka-angka. Karena kita tau kl format flagnya agrihack{}, maka dapat dilihat angka tersebut adalah Alphabet Order. Biar cepet gw langsung buat codenya di C++.

```
#include <cstdio>

int main(){
	int array[41]={1, 7, 18, 9, 8, 1, 3, 11, 38, 28, 20, 37, 10, 
		21, 19, 20, 37, 1, 12, 6, 8, 31, 2, 30, 
		20, 37, 18, 30, 16, 18, 30, 19, 30, 14, 34, 31, 34, 28, 27, 14, 39};
	char array2[41];
	for(int i=0;i<41;i++){
			array2[i]=array[i]+96;		
	}
	for(int i=0;i<41;i++){
		printf("%c",array2[i]);
	}
}
```
Dan yg keluar ternyata

![image32](https://user-images.githubusercontent.com/66354919/140282310-7d65d9ee-f601-4256-8aef-e566790acc9d.png)

Wadooh setelah saya liat-liat lagi ternyata beberapa angkanya lebih dari 26. Saya liat di hint: numbers come after letters, underscore come after numbers, Maka mungkin setelah angka 26 (huruf z), yakni angka 27 sampai 36 merupakan numbers, dan sisanya adalah underscore dan brace.

Maka listnya menjadi seperti ini:
27=0, 28=1, 29=2, 30=3, 31=4, 32=5, 33=6, 34=7, 35=8, 36=9, 37=_, 38={, 39=}.

Sehingga setelah saya coba decode manual hasil 
**flagnya: agrihack{1t_just_alfh4b3t_r3pr3s3n74710n}**

## BabyXOR (120 pts)
Diberikan file key.txt dan enc.txt. Dilihat dari nama soalnya, cara decodenya kemungkinan menggunakan metode XOR. langsung ae kita buat python scriptnya:

```
from itertools import cycle

def repeated_xor(string,key):
	result=""
	for c, k in zip(string, cycle(key)):
		result+=chr(ord(c)^ord(k))
	return result
	
ciphertext = open("/home/ilham/Documents/WriteUp/enc.txt").read()

print repeated_xor(ciphertext,"vibonacci")
```

Dilihat dari file key.txt kita tahu bahwa key untuk mendecode ciphertext dari file enc.txt tersebut adalah “vibonacci”,sehingga setelah kita input key dengan fungsi repeated_xor, dapat flagnya:

```
agrihack{you've_learn_about_xor_______let's_moving_on}

------------------
(program exited with code: 0)
Press return to continue
```
**Flag: agrihack{you've_learn_about_xor_______let's_moving_on}**

## Password Cracker (150 pts)

Diberikan sebuah hash md5:
“0f7c017187ad3c1d50a65015de71958c”

dan servis nc 52.187.65.2 15001 

Setelah saya searching cara mendecode hash md5, ternyata hash md5 tidak dapat direverse secara sistematis/irreversible. Sehingga hal yang paling mungkin untuk mendecode/crack hash md5 adalah dengan metode bruteforce huruf, menebak, atau mencari database md5nya dari internet. Sehingga saya pun mencari database md5nya di https://crackstation.net/ Sehingga, dapatlah hasil cracknya:

![image3](https://user-images.githubusercontent.com/66354919/140283686-a25fc4d7-8aa2-4d7e-a1e6-542eda53357e.png)

Didapat “b33f”. Lalu kita input pada netcat:

```
ilham@ilham-Acer:~$ nc 52.187.65.2 15001 
[+] Silahkan login untuk terhubung ke wifi :


password : b33f
[+]Login...


	[+] Login Berhasil , anda terhubung.

	Silahkan flag anda, tuan hekel :
			agrihack{brut3f0rc3333_as_1t5_f1n33______b33f}
```

**flag: agrihack{brut3f0rc3333_as_1t5_f1n33______b33f}**

## Introduction to Cipher_Series : Matrix (150 pts)

Diberikan sebuah file gambar dengan extensi .jpg, setelah dilihat gambarnya:

![image26](https://user-images.githubusercontent.com/66354919/140282818-98fd9f9a-52f4-4e19-aa62-6a5ae9a9040d.jpg)

Jika dilihat baik-baik, maka karakter-karakter tersebut sudah merupakan karakter dari flagnya, karena terdapat huruf a,g,r,i,h,.. Dst. Lalu saya mencoba untuk menyusun ulang susunan huruf tersebut. Karena hintnya adalah matriks, saya mencoba untuk menyusun hurufnya menjadi matriks. Didapatlah susunannya:

```
a g r i h
a c k [ C
0 l u m n
4 t _ T r
4 n s p 0
5 1 t 1 0
0 0 N }
```

**Dapatlah flagnya: agrihack{C0lumn4t_Tr4nsp051t100N}**

## BabyAES : Introduction (175 pts)
Diberikan file source.py dan enc.txt. Hmmmm… Setelah kita lihat codenya, mungkin kita harus mengeksekusi source code tersebut untuk mendecode file enc.txt.

Namun, setelah dieksekusi, ternyata kita harus menginstall package pycryptodome. Langsung saja kita install packagenya:

```
ilham@ilham-Acer:~$ pip install pycryptodome

```
Setelah diinstal, kita decode enc.txt di source code tersebut:

```
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import hashlib
import os

FLAG = open("enc.txt").read()
KEY = "ajari_aku_ngehek"

aes = AES.new(KEY, AES.MODE_ECB)
cipher = aes.encrypt(pad(FLAG,16))

print(cipher)
```
Keknya masih ada yang salah sintaksnya, karena yang diperlukan adalah plaintext, kita ganti terlebih dahulu sintaks aes.encrypt() menjadi aes.decrypt(). Sehingga menjadi:

```
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import hashlib
import os

FLAG = open("enc.txt").read()
KEY = "ajari_aku_ngehek"

aes = AES.new(KEY, AES.MODE_ECB)
cipher = aes.decrypt(pad(FLAG,16))

print(cipher)
```

Kita execute:

```
agrihack{51mpl3_ECB_MODE_34b6a5}��;�,	����r�


------------------
(program exited with code: 0)
Press return to continue
```
**flag: agrihack{51mpl3_ECB_MODE_34b6a5}**

## Password Cracker 2 (175 pts)

Lagi-lagi hash md5 yaitu “535fc078a3a273070809bfdbef6018bf” dan servis nc 52.187.65.2 15002. Langsung saja kita crack md5 di database online di https://crackstation.net/

![image57](https://user-images.githubusercontent.com/66354919/140283589-ded787e8-be0c-43e3-847e-daea28c24754.png)

Dapat hasil cracknya: “sarahsue@gmail.com”. Langsung kita input di netcat:

```
ilham@ilham-Acer:~$ nc 52.187.65.2 15002
[+] Silahkan login untuk terhubung ke wifi :


password : sarahsue@gmail.com
[+]Login...


	[+] Login Berhasil , anda terhubung.

	Silahkan flag anda, tuan hekel :
			agrihack{d0n7_u53_w34k__p455w000000RD}
```
**flag: agrihack{d0n7_u53_w34k__p455w000000RD}**

## BabyXor Vol.2 (200 pts)
Diberikan file enkrip.enc, dan tanpa key. Maka cara yang paling tepat adalah dengan menggunakan xortool.

Sebelumnya kita install dulu package xortool dengan cara:

```
ilham@ilham-Acer:~$ pip install xortool
```

Setelah diinstal

```
ilham@ilham-Acer:~/Documents/WriteUp$ xortool enkrip.enc
The most probable key lengths:
   3:   8.9%
   5:   9.4%
   7:   21.5%
  10:   8.5%
  12:   6.9%
  14:   14.8%
  21:   10.9%
  24:   4.7%
  28:   7.7%
  35:   6.6%
Key-length can be 3*n
Key-length can be 7*n
Most possible char is needed to guess the key!
```

Sehingga panjang karakter dari key yang paling mungkin adalah 7, lalu
Kita cari keynya dengan klue karakter sepanjang 7 tadi:

```
ilham@ilham-Acer:~/Documents/WriteUp$ xortool enkrip.enc -l 7 -c 20
1 possible key(s) of length 7:
jaogbro

Found 1 plaintexts with 95.0%+ valid characters
See files filename-key.csv, filename-char_used-perc_valid.csv
```


Nahh, ketemu keynya: “jaogbro”, langsung saja kita decode dengan key tersebut dengan fungsi repeated_xor:

```
from itertools import cycle

def repeated_xor(string,key):
	result=""
	for c, k in zip(string, cycle(key)):
		result+=chr(ord(c)^ord(k))
	return result
	
ciphertext = open("/home/ilham/Documents/WriteUp/enkrip.enc").read()
print repeated_xor(ciphertext,"jaogbro")
```

Outputnya:

```
 Exclusive or or exclusive disjunction is a logical operation that outputs true only when inputs differ (one is true, the other is false) Here is your flag agrihack{common_character__is_space}. aku ingin begini, aku ingin begitu, la la la ini banyak sekali, semua semua semua, bair ini penuh wkwk bingung nulis apa lagi. baling baling bambu. semoga yang ini udah benar challsnya, btw biar gampang pake xortool aja he he he. kalau begitu mari kita rilis soal ini sekarang juga ges wkwkw, btw kjsabf ajll oeh  lbdk loxpd gnej mxdng l genk kn d enkl kne oe  nge new kng ke kneg jk i  e eg


------------------
(program exited with code: 0)
Press return to continue
```

**flag: agrihack{common_character__is_space}**

## Selamat Datang di RSA (300 pts)
Diberikan servis nc 52.187.65.2 15004. Langsung saja kita jalankan di terminal:

```
ilham@ilham-Acer:~/Documents/WriteUp$ nc 52.187.65.2 15004
Berapakah hasil dari 3564 mod 873?
72
Nice !

Berapakah hasil dari 123456789 pangkat 100?
141741726010355877021425242397614266850230984328921683301904823759475770823898618248937223189974698092198272832940279328576746286288246412163586040073071625403994235108484654701851813111412522017073436551977468182566355508096008844818770030666259103383549754706584982939339385133685051351671665495459484240707105912956546726469283110832013048361267258779772354758935887424049533897842706489170079845902824088981773992924362039029250020367962086497153391426082783460157920931418912062690190445848693672762290558236738881832546715962674705459956895378670356212279994168084514114818989630510464134483945722383050790176271852886739698177596517655470069835676583069071363091251912629058336230389234503573930897227480759410337695934859367858714793296706039210143078982981706101059862119667407317346718937443597566001
Nice !

Berapakah hasil dari (123456789 pangkat 100 ) mod 873 ?
864
Nice !

Berapakah hasil dari (123456789 pangkat 123456789 ) mod 123456789123 ?
91123197438
Nice !
```

Hasil-hasil tersebut saya dapat dari coding python:

```
print 3564%873

print pow(123456789,100)

print pow(123456789,100,873)

print pow(123456789,123456789,123456789123)
```

Lanjut lagi ke soal selanjutnya:

```
Berapakah faktor prima terbesar dari 123456789 ?
3803
Nice !
```

Hasil tersebut dari ngoding python:

```
import math
def maxPrimeFactor(n):

   while n % 2 == 0:
      max_Prime = 2
      n /= 1

   for i in range(3, int(math.sqrt(n)) + 1, 2):
      while n % i == 0:
         max_Prime = i
         n = n / i

   if n > 2:
      max_Prime = n
   return int(max_Prime)
n = 123456789
print(maxPrimeFactor(n))
```

Lanjut ke soal berikutnya:

```
Berapakah faktor prima terbesar dari 135559704094911280733379964080758441789757655164565357031817762461022711318540283640837507092589154257758810178032399821560495620555773467900061257574296756908330466265464875840801549598742096139119659989584617827179692812221698192086787482773502103191954307332626707055214505989726149117205283458968909682488 ?
```

Wooooow, besar sekalii. Kita coba input di codingan python saya sebelumnya. Hmmmm… Keliatannya outputnya gak keluar-keluar, padahal dah saya tinggal makan, ngopi, mandi. Oke, karena angkanya terlalu besar dan menghabiskan banyak memori, maka satu-satunya cara adalah dengan melihat hasilnya langsung. What u mean?? Maksudnya adalah melihat database faktorisasi prima yang beredar di internet. Setelah saya searching 7 hari 7 malem di mbah google, akhirnya ketemu website database faktorisasi prima yaitu di http://factordb.com/. 

![image28](https://user-images.githubusercontent.com/66354919/140284537-df1b2654-1d31-4e79-9a17-a99e5950839a.png)

Ternyata faktor prima terbesarnya adalah 1604413. Langsung saja input di nc:

```
Berapakah faktor prima terbesar dari 135559704094911280733379964080758441789757655164565357031817762461022711318540283640837507092589154257758810178032399821560495620555773467900061257574296756908330466265464875840801549598742096139119659989584617827179692812221698192086787482773502103191954307332626707055214505989726149117205283458968909682488 ?
1604413
Nice !
```

Lanjut soal berikutnya:

```
Berapakah modulo inverse dari 123456789(under 11562744) ?
9835599
Nice !
```

Hasil tersebut saya code dengan python karena python memiliki package pycryptodome yang dapat menghitung invers modulo, sehingga codingan saya:

```
from Crypto.Util.number import *

print inverse(123456789,11562744)
```

Soal terakhir:

```
Saya rasa basicnya cukup, mari beralih ke soal terakhir !
Silahkan pelajari tentang RSA !
Silahkan decrypt flag ini : 695112348103370063886219960435372045899986976543845881734323322240615177344
Berikut informasi yang anda perlukan : 
n =  1399574846589126407892454199585018596810722233203462210718766339216168669721
e =  65537
```

RSA… ,mari kita googling terlebih dahulu apa itu RSA. Setelah saya dapat hasilnya di mbah google, untuk mendecrypt suatu enkripsi RSA dengan cara:

![image11](https://user-images.githubusercontent.com/66354919/140284764-7ae7eb27-e80f-45d0-a9ec-8fb45fd610e1.png)

Kita terlebih dahulu harus tahu variabel-variabel yang digunakan di RSA, yaitu:
c 		<- adalah text enkripsi (dalam hal ini flag terenkripsi) 
n 		<- adalah suatu perkalian antara 2 faktor prima. 2 faktor prima tersebut ialah p dan q, sehingga:
n 		<- p.q
totient 	<- (p-1).(q-1)
e  		<- 65537 (public exponent)	
d 		<- invers modulo dari bilangan public exponent (e), under totient. Sehingga:
d 		<- invers(e,totient)
m	     	<- pow(c,d,n) atau hasil decryptionnya

Langsung saja kita buat scriptnya:

```
from Crypto.Util.number import *

flag = 695112348103370063886219960435372045899986976543845881734323322240615177344

n=1399574846589126407892454199585018596810722233203462210718766339216168669721

p=41794375358926499210005491991715885790475505971
q=33487157890737642509961701251
e=65537

totient=(p-1)*(q-1)
d = inverse(e,totient)

m = pow(flag,d,n)
print hex(m)[2:-1].decode('hex')
```

Nilai p dan q saya cari dari factordb.com:

![image18](https://user-images.githubusercontent.com/66354919/140285045-5f54adb1-9e8c-4a57-9edf-ddf2f8d7697b.png)

p=41794375358926499210005491991715885790475505971
q=33487157890737642509961701251

Sehingga output codenya:

```
agrihack{youve_learn_basic_RSA}


------------------
(program exited with code: 0)
Press return to continue
```

**flag: agrihack{youve_learn_basic_RSA}**

# Forensics
## Wheres Waldo? (100 pts)
Diberikan sebuah file wheres_waldo.png, dengan hint find strings

![image59](https://user-images.githubusercontent.com/66354919/140285146-3801f171-6bee-4607-addd-bb860b4a97d0.png)

Karena hintnya find strings, kita strings saja di terminal:

```
ilham@ilham-Acer:~/Documents/CTF$ strings wheres_waldo.png
```

dan ternyata autosolve :)

```
...
%tEXtdate:create
2020-10-25T14:28:15+00:00O
%tEXtdate:modify
2020-10-25T14:28:15+00:00>\
IEND
agrihack{oh_h3r3_15_w4LDoo__776c64}
```
**flag: agrihack{oh_h3r3_15_w4LDoo__776c64}**

## Side-by-Side (100 pts)
Diberikan file matryoshka.png. Langsung kita binwalk terlebih dahulu di terminal:

```
ilham@ilham-Acer:~/Documents$ binwalk matryoshka.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 425 x 269, 8-bit/color RGB, non-interlaced
138           0x8A            Zlib compressed data, best compression
155374        0x25EEE         Zip archive data, at least v2.0 to extract, uncompressed size: 46, name: flag.txt
155594        0x25FCA         End of Zip archive
```

Wah, ternyata ada file zip dan txt. Kita pisahkan saja dengan foremost

```
ilham@ilham-Acer:~/Documents$ foremost matryoshka.png
```

Didapat file zip berisi txt

![image19](https://user-images.githubusercontent.com/66354919/140285373-87f56f80-1eb2-41f9-85e7-caea8952264c.png)

**flag: agrihack{y0u_c4n_aPP3nd_d4t4_Aft3r_PN6_F00tEr}**

## LatihSkillBerdusta - Round 1 (120 pts)
Diberikan file round1.png.

![image61](https://user-images.githubusercontent.com/66354919/140285465-9d6e32c1-4fdd-4fd9-9353-aa0ac40b61e4.png)

Setelah direkayasa dengan gimp, tidak menghasilkan apa-apa. Hmmm, mungkin kita coba dengan menggunakan tools zsteg dari john hammond. Tapi terlebih dulu kita install toolsnya:

```
ilham@ilham-Acer:~/Documents$ gem install zsteg
```

Setelah itu untuk mendapatkan flag secara steganography, langsung saja kita zteg:

```
ilham@ilham-Acer:~/Documents/CTF$ zsteg round1.png
imagedata           .. text: "\n\n\n\t\t\t\t\t"
b2,r,lsb,xy         .. text: "0WZ{{Esv?S"
b2,rgb,lsb,xy       .. text: "agrihack{Wh1tE_w@5_th3_1MP05t3r__1_1mp05tEr_remaining}\n"
```

**Dapat flag: agrihack{Wh1tE_w@5_th3_1MP05t3r__1_1mp05tEr_remaining}**

## HEX MASTERACE - I (150 pts)
Diberikan sebuah file gambar EX MASTERACE.png dan ternyata file tersebut corrupt atau tidak bisa dibuka. Kita cek terlebih dahulu di ghex editor, ternyata header dari data biner tersebut masih bertuliskan .IPB yang seharusnya .PNG, langsung saja kita ganti:

![image27](https://user-images.githubusercontent.com/66354919/140285675-f2467768-bc46-4bd2-b219-61c04e8b54fd.png)

Menjadi:

![image24](https://user-images.githubusercontent.com/66354919/140285700-dd24e53c-fae8-4af2-a460-35ca63d2a3fb.png)

Setelah diperbaiki gambar tersebut baru bisa dibuka dan isinya:

![image15](https://user-images.githubusercontent.com/66354919/140285789-3c928ff0-43fe-48c9-942a-4cecb39254ac.png)

**flag: agrihack{h3x_ad4lah_ko3ntji_LINZ_IS_BUCYN}**

## Dark Joke (150 pts)

Diberikan file darkJoke.png. Setelah dibinwalk, tidak terdapat file-file tambahan dalam gambar tersebut. Oke, kita coba rekayasa gambar tersebut di gimp. Setelah di coba berbagai macam kemungkinan kombinasi brightness, akhirnya ketemu juga pada titik:

![image62](https://user-images.githubusercontent.com/66354919/140285887-3c0f1d41-dc96-483a-981c-1dcb7968b1ae.png)

**flag: agrihack{RGB_1_is_d4rk_not_dark3st}**

## VN (150 pts)

Diberikan sebuah file zip yang isinya voiceNote.wav. Setelah didengar ternyata hanya siraman semangat pagi yang diucapkan seorang wanita.

Karena ini soal forensic, mungkinkah suara menghasilkan gambar? Setelah searching di mbah google, Saya baru ingat bahwa suara menghasilkan spectrum gelombang. Karena itu, saya mencoba cek dengan aplikasi spek untuk melihat spectrum suara yang dihasilkan.

Setelah dilihat dan direkayasa sedikit pada gimp, maka hasilnya seperti ini:

![image50](https://user-images.githubusercontent.com/66354919/140285970-047a4c56-dc60-405d-9ef8-475b52a96260.png)

**flag: agrihack{audio_H1dd3n_1m4g3}**

## HEX MASTERACE - II (180 pts)

Diberikan sebuah file EX_2 MASTERACE.png. Ternyata file tersebut corrupt/tidak bisa dibuka. Langsung saja kita cek data binernya di ghex editor:

![image14](https://user-images.githubusercontent.com/66354919/140286060-f3fa0637-5a3c-4424-bea4-a0ceb98205f3.png)

Ternyata footer dari extensi file png nya tidak ada. Namun, footer tersebut berada di tengah-tengah dari data binernya, sehingga tinggal kita cut saja dan pindahkan ke bagian akhir.

![image7](https://user-images.githubusercontent.com/66354919/140286092-4b9cae96-0273-4053-98c1-d2620c19d0bf.png)

Dicut dan dipindahkan ke akhir:

![image33](https://user-images.githubusercontent.com/66354919/140286158-01dd5b83-3894-4844-9df7-3561e6f71fba.png)

Hasil gambarnya:

![image35](https://user-images.githubusercontent.com/66354919/140286205-0d6548ef-0f1a-4882-bf9b-03e2e3213c54.png)

**flag: agrihack{str03rtur_Sud4h_p45t1_LINZ_IS_BUCYN}**

## Sakura SmartTV (180 pts)

Diberikan sebuah file sakuraSmartTV.wav dan setelah didengar, suaranya seremmm bgt cuuuk kaya radio nyari sinyal. Mungkin spectrum suara lagi? Setelah dicek tidak ada apa-apa. Karena suaranya mirip seperti radio, maka saya searching tentang seputar radio.
Akhirnya saya menemukan SSTV (berdasarkan huruf kapital judul juga).

Jadi SSTV/SlowScan TeleVision adalah sebuah metode transmisi gambar, untuk mentransmisi dan menerima gambar statik lewat radio dalam gambar monochrome. 
Untuk softwarenya sendiri, di ubuntu terdapat QSSTV. Lalu saya play file sakuraSmartTV.wav tersebut sambil menyalakan QSSTV, sehingga hasilnya:

![image10](https://user-images.githubusercontent.com/66354919/140286298-42c35ede-48a8-4b7a-92d5-0af0f838dfe7.png)

**flag: agrihack{SlowSc4n_T3leV1si0n}**

## Kabel Hiu (200 pts)
Diberikan sebuah kabelHiu.pcapng. Dilihat dari extensi filenya dan hint dari judul, perlu software WireShark untuk mensolve soal ini. Langsung saja kita run di WireShark. (Disini saya sudah menginstall WireShark sebelumnya).

![image56](https://user-images.githubusercontent.com/66354919/140286987-aa952b2b-e5ff-41f3-9dd7-12b95934ec11.png)













