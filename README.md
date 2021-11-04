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

![image57](https://user-images.githubusercontent.com/66354919/140282715-b80ad3ad-6de6-4c52-8994-fd32ed8cfc46.png)

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





