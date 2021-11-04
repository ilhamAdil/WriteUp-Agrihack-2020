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
