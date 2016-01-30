---
layout: post
title: LED villogtatás AVR mikrochip segítségével
created: 1289041391
comments: true
---
Erre az egyszerű feladatra a legkisebb Atmega chipet használtam, annak is az egyik lábát a 8-ból. A cél nem a feladat megvalósítása, hanem egy fejlesztői környezet kialakítása volt Linux operációs rendszeren.

Maga az égető a hobbielektronika.hu leírása alapján és a tőlük vásárolt előre égetett chip alapján lett összerakva, ami tulajdonképpen egy stk500v2 programozó klónja. Így a tyúk és a tojás esetét nem kellett megoldanom, tehát hogyan írjunk egy olyan chipet, ami másik chipeket tud majd írni. A cucc USB-s, annyi módosítással, hogy én nem USB portról táplálom, nehogy leégjen a drága alaplap a hobbim miatt.

Itt a led villogó forráskódja, ebből bármilyen komolyabb stuff elkezdhető:

<code>
#define F_CPU 10000000UL
#include <avr/io.h>
#include <util/delay.h>

void delayms(uint16_t millis) {
  while ( millis ) {
    _delay_ms(1);
    millis--;
  }
}

int main(void) {
  DDRB |= 1<<PB0; /* set PB0 to output */
  while(1) {
    PORTB &= ~(1<<PB0); /* LED on */
    delayms(100);
    PORTB |= 1<<PB0; /* LED off */
    delayms(900);
  }
  return 0;
}

</code>

És a hozzá tartozó Makefile, benne a chip típusával, ami most attiny2313:

<code>
CC=avr-gcc
CFLAGS=-g -Os -Wall -mcall-prologues -mmcu=$(AVRPART)
AVRPART=attiny2313
OBJ2HEX=avr-objcopy 
AVRDUDE=avrdude
TARGET=ledvillogo

program : $(TARGET).hex
#	$(AVRDUDE) -p $(AVRPART) -c stk500v2 -P avrdoper -e -U flash:w:$(TARGET).hex
	./uisp -dprog=stk500 -dserial=/dev/ttyACM0 -dpart=ATtiny2313 --erase --upload --verify if=$(TARGET).hex
%.obj : %.o
	$(CC) $(CFLAGS) $< -o $@

%.hex : %.obj
	$(OBJ2HEX) -R .eeprom -O ihex $< $@

clean :
	rm -f *.hex *.obj *.o
</code>

A "make program" parancsot kiadva azonnal a chipre írja a programot, tápra kapcsolva pedig már villog is a led.
