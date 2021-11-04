# WriteUp-Agrihack-2020

# Binary Exploitation
## 1. FLOW 1 (100 Pts)

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

## 1. Basic64 (100 pts)
Diberikan file enc.txt dan dilihat isinya:

![image22](https://user-images.githubusercontent.com/66354919/140281216-b8237a0a-cfcb-4de1-8c99-9808ea6cd34b.png)

Karena hintnya adalah base64, langsung saja kita decode base64 tersebut
```
In [4]: import base64

In [5]: base64.b64decode("YWdyaWhhY2t7YmFzaWNfdG9fa25vd19iYXNlNjR9")
Out[5]: 'agrihack{basic_to_know_base64}'
```
**sehingga flagnya: agrihack{basic_to_know_base64}**

## 2. Introduction to Cipher_Series : ROMAN (120 pts)
Diberikan file caesar.png:

![image60](https://user-images.githubusercontent.com/66354919/140281378-d4b26a0b-1721-4c08-bfa6-8ab725a9da89.png)

Dilihat dari nama filenya, Isinya ciphertext yang kemungkinan caesarian cipher. Kita coba decode caesarian cipher tersebut di rumkin.com dengan percobaan shift dari 1 sampai 26.

![image38](https://user-images.githubusercontent.com/66354919/140281866-15332576-b255-417e-8cbf-0169d2a2a1fb.png)

**Dapet flagnya: agrihack{Gaius_Julius_Caesar}**

## 3. Introduction to Cipher_Series : GiovanBattistaBellaso (120 pts)

Diberikan file enc.txt dan gambar seorang tokoh yang megang kunci dengan tambahan kata vibonacci. Hmmm… sepertinya kunci tersebut digunakan untuk mendecode ciphertext tersebut.

![image30](https://user-images.githubusercontent.com/66354919/140281985-95497aaa-210d-4969-81e0-16512754c421.png)

Setelah saya searching apa itu Giovan Battista Bellaso di sepuh google, ternyata tokoh tersebut yang menciptakan sandi vinegere. Langsung saja kita decode vinegere cipher di rumkin.com dengan key vibonacci.

![image6](https://user-images.githubusercontent.com/66354919/140282076-4c740b5e-79a7-46ae-94d6-f08b3377c6b9.png)

**Dapetlah flagnya: agrihack{Vignere_Ch1p3r’s_H3r3}**

## 4. Introduction to Cipher_Series : Talking (120 pts)

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

## 5. BabyXOR (120 pts)
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

## 6. Password Cracker (150 pts)

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

## 7. Introduction to Cipher_Series : Matrix (150 pts)

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

## 8. BabyAES : Introduction (175 pts)
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

## 9. Password Cracker 2 (175 pts)

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

## 10. BabyXor Vol.2 (200 pts)
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

## 11. Selamat Datang di RSA (300 pts)
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
## 1. Wheres Waldo? (100 pts)
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

## 2. Side-by-Side (100 pts)
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

## 3. LatihSkillBerdusta - Round 1 (120 pts)
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

## 4. HEX MASTERACE - I (150 pts)
Diberikan sebuah file gambar EX MASTERACE.png dan ternyata file tersebut corrupt atau tidak bisa dibuka. Kita cek terlebih dahulu di ghex editor, ternyata header dari data biner tersebut masih bertuliskan .IPB yang seharusnya .PNG, langsung saja kita ganti:

![image27](https://user-images.githubusercontent.com/66354919/140285675-f2467768-bc46-4bd2-b219-61c04e8b54fd.png)

Menjadi:

![image24](https://user-images.githubusercontent.com/66354919/140285700-dd24e53c-fae8-4af2-a460-35ca63d2a3fb.png)

Setelah diperbaiki gambar tersebut baru bisa dibuka dan isinya:

![image15](https://user-images.githubusercontent.com/66354919/140285789-3c928ff0-43fe-48c9-942a-4cecb39254ac.png)

**flag: agrihack{h3x_ad4lah_ko3ntji_LINZ_IS_BUCYN}**

## 5. Dark Joke (150 pts)

Diberikan file darkJoke.png. Setelah dibinwalk, tidak terdapat file-file tambahan dalam gambar tersebut. Oke, kita coba rekayasa gambar tersebut di gimp. Setelah di coba berbagai macam kemungkinan kombinasi brightness, akhirnya ketemu juga pada titik:

![image62](https://user-images.githubusercontent.com/66354919/140285887-3c0f1d41-dc96-483a-981c-1dcb7968b1ae.png)

**flag: agrihack{RGB_1_is_d4rk_not_dark3st}**

## 6. VN (150 pts)

Diberikan sebuah file zip yang isinya voiceNote.wav. Setelah didengar ternyata hanya siraman semangat pagi yang diucapkan seorang wanita.

Karena ini soal forensic, mungkinkah suara menghasilkan gambar? Setelah searching di mbah google, Saya baru ingat bahwa suara menghasilkan spectrum gelombang. Karena itu, saya mencoba cek dengan aplikasi spek untuk melihat spectrum suara yang dihasilkan.

Setelah dilihat dan direkayasa sedikit pada gimp, maka hasilnya seperti ini:

![image50](https://user-images.githubusercontent.com/66354919/140285970-047a4c56-dc60-405d-9ef8-475b52a96260.png)

**flag: agrihack{audio_H1dd3n_1m4g3}**

## 7. HEX MASTERACE - II (180 pts)

Diberikan sebuah file EX_2 MASTERACE.png. Ternyata file tersebut corrupt/tidak bisa dibuka. Langsung saja kita cek data binernya di ghex editor:

![image14](https://user-images.githubusercontent.com/66354919/140286060-f3fa0637-5a3c-4424-bea4-a0ceb98205f3.png)

Ternyata footer dari extensi file png nya tidak ada. Namun, footer tersebut berada di tengah-tengah dari data binernya, sehingga tinggal kita cut saja dan pindahkan ke bagian akhir.

![image7](https://user-images.githubusercontent.com/66354919/140286092-4b9cae96-0273-4053-98c1-d2620c19d0bf.png)

Dicut dan dipindahkan ke akhir:

![image33](https://user-images.githubusercontent.com/66354919/140286158-01dd5b83-3894-4844-9df7-3561e6f71fba.png)

Hasil gambarnya:

![image35](https://user-images.githubusercontent.com/66354919/140286205-0d6548ef-0f1a-4882-bf9b-03e2e3213c54.png)

**flag: agrihack{str03rtur_Sud4h_p45t1_LINZ_IS_BUCYN}**

## 8. Sakura SmartTV (180 pts)

Diberikan sebuah file sakuraSmartTV.wav dan setelah didengar, suaranya seremmm bgt cuuuk kaya radio nyari sinyal. Mungkin spectrum suara lagi? Setelah dicek tidak ada apa-apa. Karena suaranya mirip seperti radio, maka saya searching tentang seputar radio.
Akhirnya saya menemukan SSTV (berdasarkan huruf kapital judul juga).

Jadi SSTV/SlowScan TeleVision adalah sebuah metode transmisi gambar, untuk mentransmisi dan menerima gambar statik lewat radio dalam gambar monochrome. 
Untuk softwarenya sendiri, di ubuntu terdapat QSSTV. Lalu saya play file sakuraSmartTV.wav tersebut sambil menyalakan QSSTV, sehingga hasilnya:

![image10](https://user-images.githubusercontent.com/66354919/140286298-42c35ede-48a8-4b7a-92d5-0af0f838dfe7.png)

**flag: agrihack{SlowSc4n_T3leV1si0n}**

## 9. Kabel Hiu (200 pts)
Diberikan sebuah kabelHiu.pcapng. Dilihat dari extensi filenya dan hint dari judul, perlu software WireShark untuk mensolve soal ini. Langsung saja kita run di WireShark. (Disini saya sudah menginstall WireShark sebelumnya).

![image56](https://user-images.githubusercontent.com/66354919/140286987-aa952b2b-e5ff-41f3-9dd7-12b95934ec11.png)

Ternyata telah banyak user yang masuk ke situs tersebut, namun karena disoal yang ditanyakan adalah user friska. Kita cari username yang mirip friska. Ditemukan username “friezka_mo3t” yang paling mirip, dan passwordnya decrypted dalam base64 yakni “aHR0cF9pel9uMHRfc2VjdXIzX2FueW0wcjM=”. Kita decode saja di python:

```
In [1]: import base64

In [2]: base64.b64decode("aHR0cF9pel9uMHRfc2VjdXIzX2FueW0wcjM=")
Out[2]: 'http_iz_n0t_secur3_anym0r3'
```

**flag: agrihack{http_iz_n0t_secur3_anym0r3}**

## 10. HEX MASTERACE - III (250 pts)

Diberikan sebuah file EX_3.png. Dan filenya juga tidak bisa dibuka alias corrupt. Langsung saja kita cek di ghex editor:

![image31](https://user-images.githubusercontent.com/66354919/140287557-1d69bd2d-a0d0-41b9-9772-11c512ee452b.png)

![image23](https://user-images.githubusercontent.com/66354919/140287588-d8cf6885-5077-4a40-8224-df031e352c86.png)

Ternyata ada yang salah pada chunk metadatanya, dimulai dari GAMA yang seharusnya gAMA, dan IDAATx yang seharusnya IDATx. Sehingga kita revisi menjadi seperti ini:

![image20](https://user-images.githubusercontent.com/66354919/140287643-20538599-31f1-4f6b-bb0e-733e1a0ed379.png)

Setelah direvisi, kita buka gambarnya dan hasilnya:

![image52](https://user-images.githubusercontent.com/66354919/140287690-554b3738-b3a0-46fa-8916-7951bed87ce6.png)

**flag : agrihack{m3tad4ta_fri3ndly_LINZ_IS_BUCYN_PAR4HhH}**

KeyLogger (250 pts)
Diberikan sebuah file keyLogger.pcapng. Karena file tersebut .pcap langsung saja kita buka di wireshark dan hasilnya:

![image25](https://user-images.githubusercontent.com/66354919/140287943-5f24d7f2-8a11-4392-8b60-1c9242f88c84.png)

Wow, penuh dengan protocol USB. Setelah saya searching di google, ini merupakan record dari pengetikan keyboard melalui protokol USB. Saya urut terlebih dahulu list dari source 2.6.1 yang merupakan aksi pengetikkan. Lalu saya tambahkan filter ((usb.transfer_type == 0x01) && (frame.len == 72)) && !(usb.capdata == 00:00:00:00:00:00:00:00)&&!(usb.capdata == 02:00:00:00:00:00:00:00) untuk menghasilkan list yang penting-penting saja, sehingga hasilnya:

![image2](https://user-images.githubusercontent.com/66354919/140288012-8d7315b3-1b7d-4de6-82ac-00229aaac7bb.png)

Untuk mengetahui karakter dari penginputan, dapat dilihat pada Leftover Capture data. Seluruh list tersebut masing-masing berisi leftover data, mulai dari 04 dan seterusnya. 

Untuk mengubah data dari leftover (USAGE ID), maka kita lihat terlebih dulu di tabel HID USAGES USB. 

![image8](https://user-images.githubusercontent.com/66354919/140288036-6f45ae59-b075-42a3-bb35-68280fcc9946.png)

Untuk 04 adalah karakter a, sehingga kita tau ini adalah flag agrihack{..}

![image12](https://user-images.githubusercontent.com/66354919/140288079-2bc62690-3084-4c5d-8289-9974207c995b.png)

Untuk leftover bagian ini, diawali dengan 02. Sesuai dari tabel, jika diawali 02 maka karakter yang digunakan adalah karakter 2nd/ karakter kapital. Lalu saya kumpulkan leftover-leftover dari list tersebut secara manual:

```
04 0a 15 0c 0b 04 06 0e 2f(2) 18(2) 16(2) 05(2) 2d(2) 0e(2) 20 1c 16(2) 17 15 27 0e 20 20 20 2d(2) 08(2) 11 0d 27 1c 08 15 1d 30(2)
```

Untuk (2) itu merupakan huruf kapital/2nd karakter dari tabel HID USAGE tables. Lalu saya ubah menjadi character secara manual dengan melihat tabel:

```
agrihack{USB_K3yStr0k333_Enj0yerz}
```

flag: agrihack{USB_K3yStr0k333_Enj0yerz}

## 11. HEX MASTERACE - IV (300 pts)
Diberikan file gambar rusak lagi EX_4 MASTERACE.png. Langsung saja kita perbaiki di ghex editor:

![image63](https://user-images.githubusercontent.com/66354919/140288280-0eee7fac-3d65-4c43-b7bf-6b20e9d8f8c2.png)

Sepertinya, data hex diantara .PNG dan .IHDR kelebihan satu data 00 dan urutannya masih berantakan. Sehingga kita perbaiki menjadi:

![image5](https://user-images.githubusercontent.com/66354919/140288319-f88f0051-dbd9-4892-bd0f-1719283f0c34.png)

Lanjut lagi cek data setelahnya:

![image58](https://user-images.githubusercontent.com/66354919/140288352-5ca9d882-a4f2-472d-81cd-53533204dfe1.png)

Ternyata data chunk sebelum IDATx kelebihan satu hex. Kita hapus saja salah satu datanya sehingga menjadi:

![image46](https://user-images.githubusercontent.com/66354919/140288409-bcc5c2b0-27f2-4066-9478-6b21c7f6aa19.png)

Setelah diperbaiki semua, kita bisa lihat hasil gambarnya:

![image29](https://user-images.githubusercontent.com/66354919/140288431-e334cdfa-7028-49a3-a634-d61c955bf54b.png)

 **flag: agrihack{m3t4data_m45tery_KASIAN_AH_LINZ}**
 
 # Reversing
 ## 1. baby-C-0x00 (100 pts)
 
Untuk mengerjakan soal reversing, kita download terlebih dulu IDA PR0. Disini yang saya download IDA PRO versi 6.8, sebelumnya kita harus install wine terlebih dahulu, baru bisa install IDA PRO :)

Oke langsung saja kita cek terlebih dahulu tipe file baby-c-0x00.exe nya:

```
ilham@ilham-Acer:~/Documents/WriteUp$ file baby-c-0x00
baby-c-0x00: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=f905d23673179d7e192c6a4803b05a7c126ee9fd, not stripped
```

Karena tipenya 64-bit, kita jalankan reversingnya di IDA PRO 64-bit dan hasil pseudocodenya (autosolve gann):

```
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int result; // eax@3
  __int64 v4; // rdx@3
  char s1; // [sp+10h] [bp-30h]@1
  __int64 v6; // [sp+38h] [bp-8h]@1

  v6 = *MK_FP(__FS__, 40LL);
  printf("input: ", argv, argv);
  __isoc99_scanf("%32s", &s1);
  if ( !strcmp(&s1, "31337") )
    puts("Congratz!\nHere is your flag: agrihack{yeah_this_is_easier_than_C_0x??_series}");
  result = 0;
  v4 = *MK_FP(__FS__, 40LL) ^ v6;
  return result;
}
```

**flag: agrihack{yeah_this_is_easier_than_C_0x??_series}**

## baby-C-0x01 (120 pts)
Diberikan file exe 64-bit baby-C-0x01.exe, kita lakukan reversing di IDA PRO dan langsung saja generate pseudocodenya:

```
 v21 = *MK_FP(__FS__, 40LL);
  printf("input: ", argv, argv);
  __isoc99_scanf("%32s", &s);
  if ( strlen(&s) != 16 )
  {
    puts("invalid length.");
    exit(0);
  }
  if ( v11 != '_' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v14 != 'f' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v13 != v11 || v18 != v11 )
  {
    puts("nope.");
    exit(0);
  }
  if ( v15 != 'i' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v16 != 'n' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v17 != 'd' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v19 != 'm' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v20 != 'e' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v6 != 'h' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v12 != 'u' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v9 != 'a' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v8 != 'a' || v10 != v9 )
  {
    puts("nope.");
    exit(0);
  }
  if ( s != 'w' )
  {
    puts("nope.");
    exit(0);
  }
  if ( v7 != 'o' )
  {
    puts("nope.");
    exit(0);
  }
  printf("Congratz!\nHere is your flag: agrihack{%s}\n", &s);
  result = 0;
  v4 = *MK_FP(__FS__, 40LL) ^ v21;
  return result;
}
```

Disini saya telah mengganti data pada conditional dari hex menjadi char.
Dilihat dari codenya kita bisa tahu bahwa input yang harus dimasukkan adalah 16 karakter, dan merupakan data dari s, v6 sampai v20.

Setelah diurutkan, input untuk passwordnya adalah: “whoaaa_u_find_me”.
Langsung saja kita jalankan file exenya di terminal:

```
 ilham@ilham-Acer:~/Documents/WriteUp$ ./baby-c-0x01
input: whoaaa_u_find_me
Congratz!
Here is your flag: agrihack{whoaaa_u_find_me}
```

**flag: agrihack{whoaaa_u_find_me}**

## 2. C-0x00 (150 pts)

Diberikan file exe 64-bit c-0x00. Kita jalankan saja di IDA PRO, dan generate pseudocode:

```
v7 = *MK_FP(__FS__, 40LL);
  v5 = 0;
  printf("input: ", argv, argv);
  __isoc99_scanf("%32s", s);
  if ( strlen(s) != 20 )
  {
    puts("invalid length.");
    exit(0);
  }
  while ( v5 <= 19 )
  {
    if ( 2 * s[v5] != arr[(unsigned __int64)v5] )
    {
      puts("nope.");
      exit(0);
    }
    ++v5;
  }
  printf("Congratz!\nHere is your flag: agrihack{%s}\n", s);
  result = 0;
  v4 = *MK_FP(__FS__, 40LL) ^ v7;
  return result;
}
```

Dari codenya, character dari key berjumlah 20. Hmmm…  sepertinya kunci conditionalnya berada pada array arr, kita cek terlebih dulu isi dari array arr:

```
.data:0000000000201020 arr             dd 238, 202, 216, 198, 222, 218, 202, 190, 232, 222, 190

.data:0000000000201020                                         ; DATA XREF: main+90o

.data:0000000000201020                 dd 228, 202, 236, 202, 228, 230, 210, 220, 206
```
Karena pada conditional 2 * (input) harus = arr[i], maka (input) = arr[i] * 1/2 (atau setengah dari value setiap isi array). Sehingga inputnya dalam decimal akan menjadi seperti ini:

```
119 101 108 99 111 109 101 95 116 111 95 114 101 118 101 114 115 105 110 103
```

Kita convert saja decimal menjadi ascii di website https://www.rapidtables.com/ , didapat hasilnya:

![image44](https://user-images.githubusercontent.com/66354919/140293735-f25baee5-e2ab-4f2b-87a4-bc40160ccb65.png)

Jalankan file exenya:

```
ilham@ilham-Acer:~/Documents/WriteUp$ ./c-0x00
input: welcome_to_reversing
Congratz!
Here is your flag: agrihack{welcome_to_reversing}
```

**flag: agrihack{welcome_to_reversing}**

## Python-0x00 (150 pts)
Diberikan sebuah file py-0x00.pyc. Setelah saya membaca artikel di http://net-informations.com/python/iq/pyc.htm, file pyc secara singkat adalah file executable yang telah dicompile tetapi dalam bahasa python. Untuk menguncompile/mereverse file tersebut, diperluka tools pyc uncompiler, disini saya menggunakan tools uncompyle6 yang installasinya dari htps://pypi.org/project/uncompyle6/#files.

Setelah itu kita langsung uncompile saja file tersebut dan melihat isi pseudocodenya:

```
ilham@ilham-Acer:~/Documents/WriteUp$ uncompyle6 py-0x00.pyc
# uncompyle6 version 3.7.4
# Python bytecode 3.8 (3413)
# Decompiled from: Python 2.7.17 (default, Sep 30 2020, 13:38:04) 
# [GCC 7.5.0]
# Warning: this version of Python has problems handling the Python 3 "byte" type in constants properly.

# Embedded file name: py-0x1.py
# Compiled at: 2020-10-25 03:05:52
# Size of source mod 2**32: 575 bytes
buf = input('input: ')
bol = True
if len(buf) != 28:
    print('invalid length.')
    exit(0)
else:
    if buf[0] != 'd':
        bol = False
    else:
        if buf[1:3][::-1] != 'ce':
            bol = False
        else:
            if buf[3:6] != '0mp':
                bol = False
            else:
                if buf[6] != 'i':
                    bol = False
                else:
                    if buf[7:12] != 'le_py':
                        bol = False
                    else:
                        if ord(buf[12]) + 3 != ord('f'):
                            bol = False
                        else:
                            if buf[13:19][::-1] != '0ctyb_':
                                bol = False
                            else:
                                if buf[19:28] != 'de__9adj3':
                                    bol = False
                                elif bol:
                                    print('Congratz!\nHere is your flag: agrihack{' + buf + '}')
                                else:
                                    print('nope.')
# okay decompiling py-0x00.pyc
```

Dapat dilihat pada setiap conditionalnya, karakter berjumlah 28 dan kita harus mengurutkan string karakter tersebut untuk mendapatkan flag, sehingga setelah diurutkan akan menjadi seperti ini: 
“Dec0mpile_pyc_bytc0de__9adj3”

Lalu dari soal sebelum-sebelumnya, biasanya hasil dari kunci tersebut merupakan flagnya, jadi tidak saya jalankan codenya. Ternyata benar, karena itu, flagnya adalah:

**flag: agrihack{Dec0mpile_pyc_bytc0de__9adj3}**

## C-0x01 (200 pts)

Diberikan file executable 64-bit c-0x01. Dan setelah digenerate pseudocode di IDA PRO, hasil source codenya seperti ini:

```
v7 = *MK_FP(__FS__, 40LL);
  printf("input: ", argv, argv);
  __isoc99_scanf("%32s", s);
  if ( strlen(s) != 22 )
  {
    puts("invalid length.");
    exit(0);
  }
  for ( i = 0; i <= 21; ++i )
  {
    if ( i & 1 )
    {
      if ( s[i] != arr2[i / 2] )
      {
        puts("nope.");
        exit(0);
      }
    }
    else if ( s[i] != arr1[i / 2] )
    {
      puts("nope.");
      exit(0);
    }
  }
  printf("Congratz!\nHere is your flag: agrihack{%s}\n", s);
  result = 0;
  v4 = *MK_FP(__FS__, 40LL) ^ v7;
  return result;
}
```

Dilihat dari codenya, keynya sepanjang 22 karakter. Lalu jika dilihat lagi conditonalnya, maka code tersebut memiliki filter untuk karakter dengan index ganjil dan karakter dengan index genap:

```
 for ( i = 0; i <= 21; ++i )
  {
    if ( i & 1 ) //Jika karakter berindex ganjil
    {
      if ( s[i] != arr2[i / 2] )
      {
        puts("nope.");
        exit(0);
      }
    }
    else if ( s[i] != arr1[i / 2] ) //jika karakter berindex genap
    {
      puts("nope.");
      exit(0);
    }
  } 
```

Lalu kita lihat isi dari arr2 dan arr1

```
  data:0000000000201020 arr1            dd 110, 99, 95, 110, 108, 2 dup(115), 107, 119, 108, 107

.data:0000000000201020                                         ; DATA XREF: main+A8o

.data:0000000000201020                 dd 5 dup(0)

.data:0000000000201060                 public arr2

.data:0000000000201060 ; _DWORD arr2[11]

.data:0000000000201060 arr2            dd 105, 51, 2 dup(97), 121, 105, 95, 48, 97, 115, 49
```

Dilihat isi dari masing-masing array setelah saya ubah ke decimal, untuk value a.dup(), setelah saya searching di google ternyata a.dup() menduplikat isi array sebanyak a kali. Sehingga isi arraynya menjadi seperti ini:

```
arr1: 110, 99, 95, 110, 108, 115, 115, 107, 119, 108, 107  //index genap

arr2: 105, 51, 97, 97, 121, 105, 95, 48, 97, 115, 49 //index ganjil
```

Sehingga setelah diurutkan selang-seling (genap-ganjil) hasilnya menjadi seperti ini:

```
Key: 110, 105, 99, 51, 95, 97, 110, 97, 108, 121, 115, 105, 115, 95, 107, 48, 119, 97, 108, 115, 107, 49
```

Jika diubah ke ASCII dari decimal tersebut, terbentuk:

![image43](https://user-images.githubusercontent.com/66354919/140294095-c83dcf73-cb28-4620-ba83-e209e66e1036.png)

Karena sudah ketemu, kita jalankan file exenya:

```
ilham@ilham-Acer:~/Documents/WriteUp$ ./c-0x01
input: nic3_analysis_k0walsk1
Congratz!
Here is your flag: agrihack{nic3_analysis_k0walsk1}
```

**flag : agrihack{nic3_analysis_k0walsk1}**

## C-0x02 (250 pts)
Diberikan sebuah servis nc 52.187.65.2 17010 dan file exe 64-bit c-0x02. Langsung saja kita generate pseudocodenya di IDA PRO:

```
v9 = *MK_FP(__FS__, 40LL);
  setup(*(_QWORD *)&argc, argv, envp);
  printf("password: ", argv);
  __isoc99_scanf("%32s", s);
  printf("key: ");
  __isoc99_scanf("%d", &v5);
  if ( strlen(s) != 23 )
  {
    puts("wrong password!");
    exit(0);
  }
  if ( s[0] * v5 != 9559 || s[0] > v5 )
  {
    puts("wrong password!");
    exit(0);
  }
  for ( i = 0; i <= 22; ++i )
  {
    v7 = s[i] ^ v5++;
    if ( v7 != arr[i] )
    {
      puts("wrong password!");
      exit(0);
    }
  }
  puts("Welcome!\n\n");
  sleep(1u);
  system("/bin/G64180024sh");
  result = 0;
  v4 = *MK_FP(__FS__, 40LL) ^ v9;
  return result;
}
```

Dari codenya kita tahu bahwa panjang karakter dari key adalah 23, lalu kita lihat lebih lanjut ke fungsi conditionalnya:

```
if ( s[0] * v5 != 9559 || s[0] > v5 )
```

Karena 9559 sudah bentuk decimal, maka untuk menentukan s[0] dan v5 adalah dengan mengetahui faktor dari 9559. Faktor yang paling mungkin agar menghasilkan karakter ASCII yang printable adalah 121 dan 79 (121*79 = 9559). Karena s[0] harus < v5, maka:

S[0] 	= 79
v5	= 121

Setelah itu kita cek lagi conditional selanjutnya:

```
for ( i = 0; i <= 22; ++i )
  {
    v7 = s[i] ^ v5++;
    if ( v7 != arr[i] )
    {
      puts("wrong password!");
      exit(0);
    }
  }
```

Ternyata s[i] di xor dengan v5 incremental, Sehingga untuk menentukan isi setiap s[i], dengan cara:

v7 		= s[i] ^ v5++    // karena v7 harus == arr[i], maka:
arr[i]	= s[i] ^ v5++	 // lalu, karena di xor, tinggal dibalik:
s[i]		= arr[i] ^ v5++
Selanjutnya kita cek terlebih dulu isi array arr

```
.data:0000000000201020 arr             dd 54, 55, 60, 35, 9, 2 dup(22), 243, 222, 235, 240, 219

.data:0000000000201020                                         ; DATA XREF: main+10Eo

.data:0000000000201020                 dd 240, 244, 216, 248, 232, 191, 190, 251, 189, 252, 235
```

ada 2dup(22), kita tinggal menduplikat decimal 22 sebanyak 2 kali.

tinggal kita tentukan nilai dari s[i], yaitu arr[i] ^ v5++, lalu hasilnya diubah ke char. Disini saya membuat source code di c++:

```
#include <cstdio>

int main(){
	int data[23]={54,55,60,35,9,22,22,243,222,235,240,219,240,244,216,248,232,191,190,251,189,252,235};

	int s[23];
	int x=121;
	for(int i=0;i<=22;i++){
		s[i]=data[i]^x++;
	}
	for(int i=0;i<=22;i++){
		printf("%c",s[i]);
	}
return 0;
}
```

Hasil outputnya:

```
OMG_this_is_ur_pa55w0rd

------------------
(program exited with code: 0)
Press return to continue
```

Sekarang kita input di nc dengan password: OMG_this_is_ur_pa55w0rd
Dan key(v5) hasil dari faktor 9559 tadi yaitu: 121

```
ilham@ilham-Acer:~/Documents/Reverse$ nc 52.187.65.2 17010
password: OMG_this_is_ur_pa55w0rd
key: 121
Welcome!


- G64180024 Shell -

>> 
```

kosong?? tentu tidak, kita bisa cek list dari file yang ada di shell server dengan command ls.

```
- G64180024 Shell -

>> ls
chall
flag.txt
run_challenge.sh
```

Wah sepertinya ketemu file flagnya, langsung kita cat saja: 

```
>> cat flag.txt
agrihack{camellia:"makasih_udah_bukain_komputer_aku<3"}
```

**flag: agrihack{camellia:"makasih_udah_bukain_komputer_aku<3"}**

# Web
## 1. Inception (100 pts)

Diberikan sebuah website http://52.187.65.2:16000/. Karena ini autosolve kita langsung inspect element aja dengan shortkey f12 :)

![image37](https://user-images.githubusercontent.com/66354919/140294606-93cc9a87-708b-485e-8cd1-f4d99ab9462d.png)

**flag: agrihack{u_are_an_inspector!_LINZ_IS_HERE}**

## 2. X-Header (100 pts)

Diberikan sebuah website http://52.187.65.2:16007/, karena hintnya adalah header, maka kita cek header dari websitenya dengan inspect element terlebih dahulu:

![image16](https://user-images.githubusercontent.com/66354919/140294706-9a350d19-e641-492c-9b39-ad608fc3b788.png)

**flag: agrihack{x-header_header_tersembunyi_LINZ_IS_HERE}**

## 3. Bipbip (125 pts)

Diberikan site http://52.187.65.2:16001/, dan isinya cuma video seorang hacker beserta tulisan biiiiiiippppppppppppp. Karena setiap website rata-rata memiliki robots exclusion standard, maka kita cek saja pada route /robots.txt:

![image34](https://user-images.githubusercontent.com/66354919/140296582-7cbdc9f2-8d72-4228-bb0e-56ee082e70e6.png)

Ternyata disini ditemukan route baru yaitu /linz-is-here, langsung saja kita kunjungi:

![image21](https://user-images.githubusercontent.com/66354919/140296626-2c869be1-3d30-491e-a5a7-196fb32c2e94.png)

**flag: agrihack{robootssssssss_a_little_information_linz_is_here}**

## 4. Chef (125 pts)

Diberikan web http://52.187.65.2:16003/, dan setelah dicek inspect elemen dan direload ulang, yang muncul hanyalah:

![image41](https://user-images.githubusercontent.com/66354919/140296702-d6dc8186-bc12-490d-b65c-4f3770016a3f.png)

Hmmm… mungkin kali ini adalah faktor dari cookiesnya, kita cek saja cookiesnya:

![image48](https://user-images.githubusercontent.com/66354919/140296759-a636fb87-91f1-459a-98f3-bd16b9bd8799.png)

Dilihat dari kata-kata di webnya, saat mereload wesbite yang muncul adalah ur not chef admin. Mungkin disini user yang terdaftar dalam cookies adalah sebagai juna. Karena itu, kita ganti valuenya menjadi “admin” terlebih dulu.

![image40](https://user-images.githubusercontent.com/66354919/140296781-126df0a3-11e2-407e-ba81-08b5e3806b70.png)

Setelah diganti ternyata saat direload keluar flagnya :)

**flag: agrihack{chef_cookie_cook_xdxdxd_LINZ_IS_HERE}**

## 5. Kingsman (150 pts)

Diberikan sebuah situs http://52.187.65.2:16002/ dan isinya:

![image9](https://user-images.githubusercontent.com/66354919/140296886-ce126298-627d-49c1-949a-a1ccc021f478.png)

Setelah di inspect elemen, ternyata tidak ada apa-apa pada htmlnya dan cookies. Saya coba klik link yang berwarna biru, hasilnya:

![image47](https://user-images.githubusercontent.com/66354919/140296912-18359f22-6fe6-444f-80f8-98f8bfa1de39.png)

Hmmm… mungkin ada value yang harus diganti. Lalu saya cek path apa saja yang ada di website ini dengan cek route /robots.txt.

![image17](https://user-images.githubusercontent.com/66354919/140296950-7510f665-ddf9-445f-ac8b-a49e3f86d836.png)

Wah, ternyata link tadi yaitu /agent-exam, harus memiliki User-agent dengan value: Kingsman Agent 0x05. Langsung saja kita ganti user-agent yang ada di browser menjadi value tersebut, dengan f12 terlebih dulu.

![image42](https://user-images.githubusercontent.com/66354919/140296990-b5d6476e-11e6-4a96-9173-5c1e11283c19.png)

Setelah kita ganti, kita coba kunjungi link biru tadi

![image4](https://user-images.githubusercontent.com/66354919/140297025-bfdfeeea-875f-4a3b-b230-44f04eed8042.png)

**Flag: agrihack{ur_now_our_agent0x05_linz_is_here}**

## 6. Redirect (150 pts)

Diberikan sebuah situs http://52.187.65.2:16004/, dan isinya:

![image13](https://user-images.githubusercontent.com/66354919/140297102-1a839d8a-7ab4-4928-b618-b1fdad37e935.png)

Setelah di inspect elemen dan cek cookies ternyata tidak ada apa-apa. Lalu saya curiga dengan situs tersebut karena pada saat awal berkunjung, kita sudah masuk ke suatu route /swipers. Mungkin saja, route atau path tersebut harus dihilangkan.

Lalu saja coba kunjungi tanpa redirect ke /swipers dengan menggunakan curl:

```
ilham@ilham-Acer:~$ curl http://52.187.65.2:16004/
<html>
	<head><title>Redirecting...</title></head>
	<h1>Redirecting...</h1>
	<body>
		<p>agrihack{R3d1reCt_N0__r3dIrEcT!ng___x999699}
	</body>
```

Ternyata ketemu flagnya
**flag : agrihack{R3d1reCt_N0__r3dIrEcT!ng___x999699}**

## 7. LSI (200 pts)
Diberikan situs http://52.187.65.2:16005/, dan dicek isinya:

![image1](https://user-images.githubusercontent.com/66354919/140297219-f5f680db-db4c-4b58-9b35-9cb18a8e5b57.png)

Setelah melihat-lihat di link hrefnya, pada secret file terdapat clue bahwa flagnya terdapat di flag.php. Lalu kita cek http://52.187.65.2:16005/list.php?page=flag.php, dan ternyata isinya tidak ada apa-apa. Mungkin dapat kita ambil flagnya dengan PHP wrapper yaitu php://filter, langsung saja kita coba:

![image45](https://user-images.githubusercontent.com/66354919/140297295-6e1acfef-f01b-4279-87c2-263dcbaa57da.png)

Dan ternyata kita dapat suatu encoded text base64:

![image51](https://user-images.githubusercontent.com/66354919/140297338-38f0d08e-2006-40d7-b781-fb533862b889.png)

Kita decode saja di terminal, dan mengubahnya kedalam format php/html, dengan cara:

```
ilham@ilham-Acer:~$ echo "PD9waHAKCS8vYWdyaWhhY2t7T09QU0lFRUUhISFfS2V0YXVhbmRlaGZpbGVyYWhhc2lhbnlhX0xJTlpfSVNfSEVSRX0KPz4=" | base64 -d
<?php
	//agrihack{OOPSIEEE!!!_Ketauandehfilerahasianya_LINZ_IS_HERE}
```

**flag: agrihack{OOPSIEEE!!!_Ketauandehfilerahasianya_LINZ_IS_HERE}**






