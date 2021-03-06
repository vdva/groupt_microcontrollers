## Bit-wise operaties en integer (deel 1)

Eerder hadden in deze cursus hebben we integer (int) gezien als datatype.
We hebben deze gebruikt voor  

* Mathematische expressies (en operators) zoals +, *, -, / en %
* Logische expressies en operators zoals &&, || en !
* Relationele expressies en operatoren zoals >,<, ...

Wat deze operatoren allemaal gemeen hebben met elkaar is dat deze met de waarde van een getal (integer in dit geval) werken (mathematische bewerkingen, vergelijkingen van getallen, combineren van vergelijkingen, ...).  

Zoals we direct gaan zien bestaan er ook operatoren om integers te manipuleren op bit-niveau.  

Dit is een vaardigheid die we nog veel gaan nodig hebben bij het werken met microcontrollers (of ander low level-programmeer-activiteiten).  
Alvorens echter deze operaties en expressies te bespreken gaan we eerst kijken hoe zo een een integer er van binnen uit ziet.  

### Duiding: integer-types heb je in verschillende groottes

Tot nu toe hebben we het type **int** gebruikt in onze code-voorbeelden, dit type heeft als kenmerken:

* **Signed type**, het kan zowel positief als negatief getal voorstellen
* Minimum **2 bytes** in het geheugen volgens C-specificatie
* Meestal echter **4 bytes** op intel-processoren
* **1 bit**, de MSB (most significant) wordt gebruikt voor het sign (negatief of positief)

Naast deze integer heb je ook een aantal andere **signed types** met verschillende groottes zoals **char, short, long, int, long en long long** die allemaal variëren in lengte (het aantal aantal bytes lengte.

### Duiding: voorlopig bekijken we enkel unsigned integers

We nemen eerst echter een andere type onder de loep, namelijk de **unsigned integer** (interne opbouw, conversies, ...)   
Bedoeling hier is te leren werken met bit-operators en de regels rond unsigned integers zijn eenvoudiger en bit-expressies zijn een pak éénvoudiger. 
Deze zijn eenvoudiger van opbouw en meer relevant voor een eerste kennismakig met bit-operatoren en expressies..  

Net zoals het type int dat we eerder gebruikt hebben in vorige hoofdstukken hebben we ook zijn tegenhanger de **unsigned int**.

```{.c}
unsigned int = 5;
```

Het unsigned integer-type bestaat in verschillende maten (aantal bytes);


|type                  |minimum           | x86               | 
|----------------------|------------------|-------------------|
| unsigned char        | 1                | 1                 |
| unsigned short       | 2                | 2                 |
| unsigned int         | 2                | 4                 |
| unsigned long        | 4                | 4                 |
| unsigned long long   | 8                | 8                 |

De byte-encoding van een unsigned char is vrij éénvoudig vergeleken met de signed varianten (die we volgende les bekijken).  

Het getal 0xAAAA

|type  | hex      |15  | 14 | 13 | 12 | 11 | 10 |  9 |  8 |  7 |  6 |  5 |  4 |  3 |  2 |  1 |  0 |
|------|----------|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
|short |0x1010    | 1  | 0  | 1  | 0  | 1  | 0  | 1  | 0  |  1 | 0  | 1  | 0  |  1 | 0  | 1  | 0  |
|char  |0x10      | -  | -  | -  | -  | -  | -  | -  | -  |  1 | 0  | 1  | 0  |  1 | 0  | 1  | 0  |


We beperken ons in deze les ook (meestal) tot de unsigned char:

* Stelt een byte voor
* Heeft geen sign-bit  
* Start met de MSB
* Eindigt met de LSB
* Elke waarde stelt een macht van 2 voor
* Dus de range van waardes is (0-255)

|positie     |7   |6   |5   |4   |3   |2    |1    |0    |
|------------|----|----|----|----|----|-----|-----|-----|
|macht 2     |128 |64  |32  |16  |8   |4    |2    |1    |
|AA (170)    |1   |0   |1   |0   |1   |0    |1    |0    |


Vanuit de bitwise operatoren de we zo direct gaan zien kan je het volgende als model nemen voor een unsigned char

### Duiding: HEX-representatie

Als we de onderliggende representatie (of encoding) gaan gebruiken - op bit-niveau -  is het handig getallen uit te drukken als hexadecimale getallan.

|base 10  |base 16 | base 2 |
|---------|--------|--------|
|0        |0       |00000000|
|1        |1       |00000001|
|2        |2       |00000010|
|3        |3       |00000011|
|4        |4       |00000100|
|5        |5       |00000101|
|6        |6       |00000110|
|7        |7       |00000111|
|8        |8       |00001000|
|9        |9       |00001001|
|10       |a       |00001010|
|11       |b       |00001011|
|12       |c       |00001100|
|13       |d       |00001101|
|14       |e       |00001110|
|15       |f       |00001111|

### Voorbeeld: een waarde uitdrukken als hex in c-code

Een unsigned char (maar ook andere integer-variante) kan hexadecimaal gerepresenteerd worden door "0x" te laten volgen door een hexadecimaal getal.   

Bijvoorbeeld ```int a = 0xA``` zal er voor zorgen dat integer a wordt geinitiaseerd met de waarde 10 (decimaal) of A (hexadecimaal)

Onderstaande code zal desgevolg ook 10 afdrukken.

```{.c}
#include <stdio.h>

void main() 
{
    int value_ten = 0xa;
    printf("%i\n",value_ten);
}
```

Onderstaande code toont ook aan dat de hex-representatie de zelfde waarde geeft als  de decimale representatie...

```{.c}
#include <stdio.h>

void main() 
{
    if(10 == 0xa) {
       printf("10 == 0xa");
    }
}
```

### Voorbeeld: printf in hex-formaat

De printf-functie geeft je ook de mogelijkheid om de waarde als hexadecimaal getal af te drukken.  

Dit kan je gebruiken door de placeholder x te gebruiken (ipv i die we tot nog toe hebben gebruikt)

```{.c} 
#include <stdio.h>

void main() 
{
    int value_ten = 10;
    printf("%i == %x\n",value_ten,value_ten);
}
```

### Voorbeeld: sizeof-operator

Vandaag beperken we ons tot de "unsigned char" om op een eenvoudige manier een introductie te geven naar het werken met bit-operatoren.  

Als je echter wil weten hoe groot andere 

In c heb je trouwens een operator (geen functie) met de naam sizeof die toelaat van de grootte van een bepaald type te verkrijgen.  

```{.c}
#include <stdio.h>

int main(void)
{
	printf("sizeof(unsigned char) = %i bytes\n", (int) sizeof(unsigned char));
	printf("sizeof(unsigned short) = %i bytes\n", (int) sizeof(unsigned short));
	printf("sizeof(unsigned int) = %i bytes\n", (int) sizeof(unsigned int));
	printf("sizeof(unsigned long) = %i bytes\n", (int) sizeof(unsigned long));
	printf("sizeof(unsigned long long) = %i bytes\n", (int) sizeof(unsigned long long));
    return 0;
}
```

```
$ ./size_of_signed_ints
sizeof(unsigned char) = 1 bytes
sizeof(unsigned short) = 2 bytes
sizeof(unsigned int) = 4 bytes
sizeof(unsigned long) = 8 bytes
sizeof(unsigned long long) = 8 bytes
$
```


### Duiding: Bitwise operatoren vs logische operatoren

Tot nog toe hebben we logische operatoren gezien zoals &&, || en !, deze hadden als eigenschappen:

* Een integer-operand met waarde 0 wordt beschouwd als een **logische 0**
* Elke integer-operand met een waarde <> 0 wordt beschouwd als een **logische 1**
* Je krijgt een logische uitkomst, dus enkel 0 of 1 als uitkomst

De operatoren die we vandaag bekijken noemen we bitwise operatoren.  
Met bit-wise willen we zeggen dat deze op bit-niveau opereren, ipv de operanden als 0 en 1 te gaan beschouwen gaan deze operator elke bit mat elkaar kan vergelijken.

### Voorbeeld: verschil tussen logische en bitwise operatoren

We kunnen dit best met een stukje code gaan vergelijken

```{.c}
#include <stdio.h>
void main()
{
	printf("2 && 2 = %i\n",2 && 2);
	printf("2 & 2 = %i\n", 2 & 2);	
}
```
Als je dit uitvoert krijg je de volgende output

```
$ gcc twoandtwo.c -o twoandtwo
$ ./twoandtwo
2 && 2 = 1
2 & 2 = 2 

```

Bij de eerste statement (&&) gaat men enkel kijken of de integer <> is van 0.  
Zo ja wordt deze als een 1 bekeken.  

Het resultaat van een logische AND toe te passen is heeft dan ook het getal 1 als resultaat.  

|**&&**                 |**2**|**1**|**0**|
|-----------------------|-----|-----|-----|
|**2 wordt 1**          |0    |0    |1    |
|**2 wordt 1**          |0    |0    |1    | 
|**2 && 2 == 1 && 1 = **|0    |0    |1    |

Bij de bitwise operator gaat deze de bits individueel met elkaar gaan matchen en krijgen we het getal 2 als resultaat

|**&**          |**2**|**1**|**0**|
|---------------|-----|-----|-----|
|**2 blijft 2** |0    |1    |0    |
|**2 blijft 2** |0    |1    |0    | 
|**2 & 2**      |0    |1    |0    |

### Voorbeeld: verschil tussen logische en bitwise operatoren (2)

Nog een ander voorbeeld is de combinatie 5 en 2.  
Hier is het verschil tussen beide nog opvallender (1 en 0),.

```{.c}
#include <stdio.h>
void main()
{
	printf("5 && 2 = %i\n",5 && 2);
	printf("5 & 2 = %i\n", 5 & 2);	
}
```
Als je dit uitvoert krijg je de volgende output:

```
$ gcc twoandtwo.c -o twoandtwo
$ ./twoandtwo
5 && 2 = 1
5 & 2 = 0 
```
Bij de logische operator krijgen we opnieuwe 1 als resultaat:

|**&&** |**2**|**1**|**0**|
|-------|-----|-----|-----|
|**5**  |0    |0    |1    |
|**2**  |0    |0    |1    | 
|**1**  |0    |0    |1    |

Bij de bitwise worden de individuele bits vergeleken.  
Gezien er op elke positie bij beide getallen minstens één 0 voorkomt is het resultaat van deze combinatie ook 0.

|**&**  |**2**|**1**|**0**|
|-------|-----|-----|-----|
|**5**  |1    |0    |1    |
|**2**  |0    |1    |0    | 
|**2**  |0    |0    |0    |

> Dit kan gevaarlijk zijn als je dit in een if-conditie of loop zou gebruiken (& ipv &&)

### Overzicht: bitwise operatoren  

Naast & heb je hier een overzicht van all bitwise operatoren:

| Operator | Betekenis             | Voorbeeld    | Resultaat (per bit-positie)                       |
|----------|-----------------------|--------------|---------------------------------------------------|
| &        | AND                   | x & y        | 1 als beide 1 zijn                                |
| &#124;   | OR                    | x &#124; y   | 1 als er 1 van de 2 een 1 voorkomt                |
| ^        | XOR                   | x ^ y        | 1 als enkel 1 operator 1 bevat                    |
| ~        | NOT                   | ~x           | 1 als x 0 is                                      |

> Binaire getallen worden vanaf hier bits genoemd (binary digits)  
> Als we in deze tekst bits vergelijken, vergelijken we bits op de zelfde positie bij 2 integers

### Voorbeeld: Bitwise operator & (and)

& (and) zal:  
* in 0 resulteren als 1 van beide bits gelijk zijn aan 0  
* in 1 als beide bits 1 zij

zoals in onderstaand waarheidstabel

| A | B | S  |
|:--|:--|:---|
| 0 | 0 | 0  |
| 0 | 1 | 0  |
| 1 | 0 | 0  |
| 1 | 1 | 1  |

Toegepast op een concreet voorbeeld zie je dat enkel de binaire digits (

| &         | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|-----------|---|---|---|---|---|---|---|---|
|**5B**     | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 1 |
|**CA**     | 1 | 1 | 0 | 0 | 1 | 0 | 1 | 0 |
|**4A**     | 0 | 1 | 0 | 0 | 1 | 0 | 1 | 0 |

Code:

```{.c}
#include <stdio.h>
void main()
{
	printf("0x5B & 0xCA = %x\n", 0x5B & 0xCA);	
}
```
Als je dit uitvoert krijg je de volgende output

```
$ gcc example_bit_and.c -o example_bit_and
$ ./example_bit_and
0x5B & 0xCA = 4a
$ 
```

### Voorbeeld: Bitwise operator | (or)

| (or) zal in 1 resulteren als minstens 1 van opereranden gelijk is aan 1 volgens de waarheidstabel.

| A | B | S  |
|:--|:--|:---|
| 0 | 0 | 0  |
| 0 | 1 | 1  |
| 1 | 0 | 1  |
| 1 | 1 | 1  |

Voorbeeld:

| &#124;   | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|----------|---|---|---|---|---|---|---|---|
|**5B**    | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 1 |
|**CA**    | 1 | 1 | 0 | 0 | 1 | 0 | 1 | 0 |
| **DB**   | 1 | 1 | 0 | 1 | 1 | 0 | 1 | 1 | 

Code:

```{.c}
#include <stdio.h>
void main()
{
	printf("0x5B | 0xCA = %x\n", 0x5B | 0xCA);	
}
```
Als je dit uitvoert krijg je de volgende output

```
$ gcc example_bit_or.c -o example_bit_or
$ ./example_bit_or
0x5B | 0xCA = db
$ 
```

### Voorbeeld: Bitwise operator ^ (xor)

^ (xor) zal in 1 resulteren als 1 van beide operanden 1, anders zal de uitslag altijd 0 zijn

| A | B | S  |
|:--|---|:---|
| 0 | 0 | 0  |
| 0 | 1 | 1  |
| 1 | 0 | 1  |
| 1 | 1 | 0  |

Zoals je zien in de laatste rij resulteren de kolommen waar beide bits gelijk zijn in een 0.

| ^        | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|----------|---|---|---|---|---|---|---|---|
|**5B**    | 0 | 1 | 0 | 1 | 1 | 0 | 1 | 1 |
|**CA**    | 1 | 1 | 0 | 0 | 1 | 0 | 1 | 0 |
|**DB**    | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 1 |

Hetzelfde voorbeeld in code bevestigd de tabel:  

```{.c}
#include <stdio.h>
void main()
{
	printf("0x5B ^ 0xCA = %x\n", 0x5B ^ 0xCA);	
}
```

Als je dit uitvoert krijg je de volgende output

```
$ gcc example_bit_xor.c -o example_bit_xor
$ ./example_bit_xor
0x5B ^ 0xCA = db
$ 
```

### Voorbeeld: Bitwise operator ~ (invertor)

De laatste bitwise operator die we zien is de invertor.  
Dit is een unitaire operator (slechts 1 operand).  

```{.c}
#include <stdio.h>
void main()
{
	printf("~0xAA = %x\n", 0x55);	
}
```

Deze operator zal dus de waarde gaan om van elke individuele bit

|          | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|----------|---|---|---|---|---|---|---|---|
|**0xAA**  | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 |
|**~0xAA** | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 |

> **Nota:**  
> Bij de laatste voorbeelden gaan we zien dat we de xor kunnen gebruiken voor individuele bits te inverteren

### Duiding: Bit-shifting

Een 2de soort van bit-operatoren zijn de bit-shift-operatoren

| Operator | Betekenis             | Voorbeeld    | Resultaat (per bit-positie)                                        |
|----------|-----------------------|--------------|--------------------------------------------------------------------|
| <<       | shift links           | x << y       | Elke bit van x wordt y posities naar links (richting MSB) geduwd   |
| >>       | shift rechts          | x >> y       | Elke bit van x wordt y posities naar rechts (richting LSB geduwd   |

Belangrijke kenmerken van deze operatoren (logical shift):

* De bits die aan de rand van je getal zitten verdwijnen (worden bijna letterlijk weggeduwd)   
    * 00000101 >> 1 zal resulteren in 00000010, de LSB ben je kwijt  
    * 10100000 << 1 zal resulteren in 01000000, de RSB ben je kwijt
* De nieuwe bits die worden ingeschoven zijn 0  

> Dit gedrag is trouwens niet 100 % overeenkomstig bij signed getallen waar een arithmatische shift wordt uitgevoerd.  
> Hier komen we later in de cursus nog op terug.   

### Voorbeeld <<

|      expressie|   base 10|   base 16|    base 2|
|---------------|----------|----------|----------|
|       0xa << 0|        10|         a|  00001010|
|       0xa << 1|        20|        14|  00010100|
|       0xa << 3|        80|        50|  01010000|
|       0xa << 4|       160|        a0|  10100000|

> Bemerking: de getallen worden in dit geval ook vermenigvuldigd met 2

```{.c}
#include <stdio.h>
void main()
{
    printf("%x\n",0xa << 0);	
    printf("%x\n",0xa << 1);	
    printf("%x\n",0xa << 3);	
    printf("%x\n",0xa << 4);	
}
```


### Voorbeeld >>

|      expressie|   base 10|   base 16|    base 2|
|---------------|----------|----------|----------|
|      0xa0 >> 0|       160|        a0|  10100000|
|      0xa0 >> 1|        80|        50|  01010000|
|      0xa0 >> 3|        20|        14|  00010100|
|      0xa0 >> 4|        10|         a|  00001010|

> Bemerking: de getallen worden in dit geval ook gedeeld door 2

```{.c}
#include <stdio.h>
void main()
{
    printf("%x\n",0xa >> 0);	
    printf("%x\n",0xa >> 1);	
    printf("%x\n",0xa >> 3);	
    printf("%x\n",0xa >> 4);	
}
```
### Voorbeeld: >> wegshiften van bits

|      expressie|   base 10|   base 16|    base 2|
|---------------|----------|----------|----------|
|      0x0a >> 0|        10|         a|  00001010|
|      0x0a >> 1|         5|         5|  00000101|
|      0x0a >> 3|         1|         1|  00000001|
|      0x0a >> 4|         0|         0|  00000000|

### Voorbeeld: << wegshiften van bits

|      expressie|   base 10|   base 16|    base 2|
|---------------|----------|----------|----------|
|      0xa0 << 0|       160|        a0|  10100000|
|      0xa0 << 1|        64|        40|  01000000|
|      0xa0 << 3|         0|         0|  00000000|
|      0xa0 << 4|         0|         0|  00000000|


### Duiding: Toepassing met bitmasks

De vraag die nu overblijft, waarvoor kan je dit nu allemaal gebruiken?
Als je software schrijft voor een MCU werkt of specifieke low-level-software zoals bijvoorbeeld drivers moet je geregeld bits op een memory-locatie manipuleren.   

Je gaat als het ware integers niet meer bekijken als getallen waar je wiskundige bewerkingen met uitvoert, je gaat ze bekijken als stukjes memory, aaneenschakelingen van bits.

Om dit efficiënt toe te passen gaan we beide operatoren - bitwise- en shiftoperatoren - combineren naar een bitmask.  
Zo'n **bitmask** kan je best bekijken als een **bit-patroon** (of masker) dat je over een **integer-variabele** legt om:

* Te weten te komen wat de waarde is van een bit op een specifieke locatie
* Een specifieke bit (of meerdere tegelijkertijd) te setten (op 1 zetten)
* Een specifieke bit (of meerdere tegelijkertijd) te clearen (op 0 zetten)
* Een specifieke bit (of meerdere tegelijkertijd) te togglen of alterneren
* Of eender welke combinatie van de 4 voorgaande mogelijkheden  

De eerste die we bekijken is het uitlezen van een bit of een specifieke locatie.

### Voorbeeld: Een bit-query met and (bit op specifiek positie lezen)

Je wil weten of een specifieke bit is geactiveerd?
Dit kan je doen door 2 operatoren te combineren & en <<

* Met ```1 << x``` kan je een bit-patroon maken dat enkel een 1 bevat op positie x
* Als je dit patroon toepast, in de onderste tabel (1 << 3 == 00000100) met een bitwise & zal die volgende operatie toepassen

Beschouw de volgende code:

```{.c}
#include <stdio.h>

void main()
{
    int test_getal= 0x5;

    if(test_getal & (1 << 2)) {
        printf("bit %i van testgetal %x is gezet\n",2,test_getal);
    } else {
        printf("bit %i van testgetal %x is niet gezet\n",2,test_getal);
    }

    if(test_getal & (1 << 5)) {
        printf("bit %i van testgetal %x is gezet\n",1,test_getal);
    } else {
        printf("bit %i van testgetal %x is niet gezet\n",1,test_getal);
    }
}
```

Volgende tabel illustreert wat er zich afspeelt in de eerste if-statement:

|                         | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|-------------------------|---|---|---|---|---|---|---|---|
| test_getal = 5          | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 1 |
| 1 << 2                  | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |
| **&**                   | & | & | & | & | & | & | & | & |
| 5 & (1 << 2)            | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 0 |

Het 1ste if-statement zal in een byte resulteren met zeker 1 bit op 1, binnen een if-statement wordt een getal <> 0 als een logische TRUE beschouwd.   

Je maakt gebruik maakt van het principe dat een **0 dominant** is bij een **&** operator:

* Overal buiten positie 2 is de bit 0, dus gegarandeerd zal dit 0 als resultaat voortbrengen
* Waar de bitmask 1 is (positie 2) zal de & zich gedragen als een buffer gate (behoudt de waarde van de bit)

Het 2de if-statement echter zal in een 0 resulteren (en binnen een if geldt dit als false)

|                         | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|-------------------------|---|---|---|---|---|---|---|---|
| test_getal = 5          | 0 | 0 | 0 | 0 | 0 | 1 | 0 | 1 |
| 1 << 5                  | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| **&**                   | & | & | & | & | & | & | & | & |
| 5 & (1 << 5)            | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |


> **Nota:**  
> Je gebruikt als ware de 2 soorten operatoren om patronen toe te passen op integers of memory-locaties.   
> Het manipuleren van individuele bits in het geheugen is 1 van de belangrijke vaardigheden voor embedded programmeren (zoals we weldra gaan zien)

### Voorbeeld: een samengestelde bitmask

We hebben dit nu toegepast om 1 enkele bit te selecteren.   
Je kan ook grotere bitmask maken, stel voor dat je wil weten dat minstens 1 positie is  


```{.c}
#include <stdio.h>

void main()
{
    int test_getal= 0xAA;

    if(test_getal &  (1 << 3) | (1 << 6) {
        printf("bit %i van testgetal %x is gezet\n",2,test_getal);
    } else {
        printf("bit %i van testgetal %x is niet gezet\n",2,test_getal);
    }
}
```

|                         | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
|-------------------------|---|---|---|---|---|---|---|---|
| test_getal = 0xA        | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 |
| (1 << 3) | (1 << 6)     | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 0 |
| **&**                   | & | & | & | & | & | & | & | & |
| 5 & (1 << 2)            | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |


### Voorbeeld: een bitmask toepassen op byte-niveau

Als voorbeeld, stel het volgende:

* Een unsigned short heeft 16 bits
* Je wil deze opsplitsen in 2 chars
    * 1 met de MSB byte
    * 1 met de LSB byte

```{.c}
#include <stdio.h>

void main()
{
   unsigned short big_number = 0xAABB;
   unsigned char ls_byte = big_number & 0x00ff;
   unsigned char ms_byte = (big_number & 0xff00) >> 8;

   printf("ms byte of number %x is %x\n",big_number,ms_byte);
   printf("ls byte of number %x is %x\n",big_number,ls_byte);
}
```
Bovenstaande code combineert in dit geval de 2 bit- en shift-operatoren:

* Voor de ls_byte is dit vrij eenvoudig:  
```
  0000000011111111  00ff
& 1010101010111011  aabb
  0000000010111011  00bb 
```
* Voor de ms_byte zijn er 2 stappen nodig:  
```
  1111111100000000  ff00
& 1010101010111011  aabb
  1010101000000000  aa00
>> 8
  0000000010101010  00aa
```

### Voorbeeld: printen welk bits zijn gezet

```{.c}
#include <stdio.h>

void main()
{
	 int test_getal;
     int i=0;

     printf("Geef een getal in: ");
     scanf("%i",&test_getal);

     while(i<8) {
         if(test_getal & 1 << i) {
            printf("bit %i van testgetal %x is gezet\n",i,test_getal);
         }
    i++;
    }
}
```

### Voorbeeld: bits setten met or (wijzigen naar 1)

Tot nu toe hadden we enkel geprobeerd een waardes te weten te kokmen.  
Je kan ook een bit in een getal wijzigen, dit is een vaardigheid dat je zeer veel nodig zal hebben voor een MCU-programma.  

```{.c}
#include <stdio.h>

void main()
{
     int a = 0x1;
     a = a | (0x1 << 4);
     printf("%x\n",a); 
}
```
In dit geval worden eerst de shift-operator uit gevoerd (dit is ook het geval zonder haakjes maar ter duidelijkheid).   

* Dit creert net zoals bij de vorige voorbeelden een bitmask (00001000).  
* Als je de |-operator toepast zal de bit op de 4 positie (van rechts)
    * wijzigen naar 1 als deze 0 is
    * 1 blijven als deze reeds 1 is

|                expressie|   base 10|   base 16|    base 2|
|-------------------------|----------|----------|----------|
|                int a = 1|         1|         1|  00000001|
|                 0x1 << 4|        16|        10|  00001000|
|   a = a &#124;(0x1 << 4)|        17|        11|  00001001|

We maken hier gebruik (in tegenstelling tot lezen van een bit) van een **|**  
Bij deze operator is de **1 dominant**, dus waar een 1 wordt gebruikt bij het masker zal de waarde worden geforceerd naar 1.  
Wanneer er een 0 staat op een specifieke locatie zal deze zich gedragen als een buffer (de originele waarde behouden).  

### Voorbeeld: bits clearen met and (wijzigen naar 0)

Een andere actie die je kan toepassen is een specifieke bit wijzigen naar 0 (ook wel de bit clearen genoemd).
Dit wordt gedaan door een geinverteerd bit-patroon toe te passen zoals hieronder:

```{.c}
#include <stdio.h>

void main()
{
     int a = 0xA;
     a = a & ~(0x1 << 1);
     printf("%x\n",a);
}
```
Dit doet het omgekeerde van voorgaand voorbeeld

* De negatie interveert het patroon 00000010 naar 11111101
* Waar binnen het patroon de bit 1 is zal deze (bij de & operatie)
    * bits die 1 zijn 1 laten
    * bits die 0 zijn 0 laten
* Maar op de positie in het patroon waar de bit 0 is zal deze
    * bits die 0 zijn 0 laten
    * bits die 1 zijn 0 maken

|                expressie|   base 10|   base 16|    base 2|
|-------------------------|----------|----------|----------|
|              int a = 0xa|        10|         A|  00001010|
|                 0x1 << 1|         2|         2|  00000010|
|              ~(0x1 << 1)|        13|         d|  11111101|
|   a = a & ~(0x1 << 1)   |         8|         8|  00001000|

### Voorbeeld: Bits togglen (of wisselen) met xor

Als laatste voorbeeld (voortgaand op de voorgaande voorbeelden) kan je ook individuele bits laten togglen.  
(togglen = laten alterneren tussen 0 en 1)   

Dit kan je eventueel doen met een invertor (~), maar deze gaat alle bits inverteren.  
In dit geval willen we dit toepassen met een bitmask, en daar komt de xor zeer goed van pas.  

Een xor heeft de eigenschap zich te gedragen als een poort die je kan configureren als: 

* ofwel een buffer (wanneer je deze als 1 configureert)  
* ofwel een invertor (wanneer je deze als 0 configureert)

Anders gezegd:

* Op de positie waar een bitmask de waarde 1 heeft zal dit het gedrag van een invertor vertonen    
  (0 ^ **1** = 1 en 1 ^ **1** = 0)  
  en de bit van het source-getal **inverteren**  
* Op de positie waar een bitmask de waarde 0 heeft zal dit het gedrag van een buffer vertonen    
  (0 ^ **0** = 0 en 1 ^ **0** = 1)   
  en de bit van het source-getal **niet wijzigen**  

```{.c}
#include <stdio.h>

void main()
{
     int a = 0x2;
     a = a ^ (0x1 << 1);
     printf("%x\n",a);

     a = a ^ (0x1 << 1);
     printf("%x\n",a);

     a = a ^ (0x1 << 1);
     printf("%x\n",a);

     a = a ^ (0x1 << 1);
     printf("%x\n",a);
}
```

Het resultaat van bovenstaande code is als volgt:  

```
$ ./example_of_toggling
0
2
0
2
$
```

De 2de bit van rechts (positie 1) alterneert telkens als we deze expressie uitvoeren.   
  
Dit is namelijk de eigenschap van een xor die we vanboven hebben beschreven.  
Als je een specifieke bit telkens opnieuw met 1 zal "xor-en" zal deze telkens van waarde wisselen (inverteren) als volgt:

* Je start met een xor tussen 2 maal dezelfde bitwaarde (op positie 1)
```
  0010 
^ 0010 
  0000 
```
Dit levert uiteindelijk een 0 op want beide bits zijn gelijk aan elkaar (en dat levert de waarde 0 op bij xor).  

* De volgende keer echter - als we opnieuw de bit 1 als mask toepassen - zal deze opnieuw 1 worden.    
```
  0000 
^ 0010 
  0010 
```
In dit geval verschillen de bits van waarde en dit geeft een 1 als resultaat bij een xor.

* En dit gedrag blijft zich herhalen bij de volgende keer dat we hetzelfde bit-patroon toepassen
```
  0010 
^ 0010 
  0000 
```
```
  0000 
^ 0010 
  0010 
```

> **Nota:**  
> Met de invertor-operator (~) kan je dit enkel op de volledige char (of integer) toepassen (zonder bitmask).

### Besluit: denk in bits

In een klassieke programmeer-curus worden het werken met bitmasks niet bij de start aangeleerd.  
Voor het programmeren van microcontrollers is dit echter een essentiële vaardigheid.   
Dit komt door het principe van "memory mapped io", hetgeen geheugen gaat koppelen aan IO-devices (later meer hierover).  

Dus belangrijk te onthouden:  

* Bitwise operatoren zijn verschillend van hun logische verwanten om dat ze het vergelijk op **bit-level** maken
* Denk bij bitwise operatoren in bits en niet aan de getal-waarde (mind-switch)
* **Dominantie** van 0 of 1
    * Bij **&** (AND) is **0** dominant dus 
           * kan je gebruiken om een bit op een positie te **clearen** (en andere bit-posities in masker op 1 zodat die niet wijzigen)
           * te lezen door enkel de bits die je wil lezen op 1 te zetten (en andere bit-posities op 0 zodat die niet rekening worden gebracht 
    * Bij **|** (OR) is **1** dominant dus kan je gebruiken om een bit op positie te **setten** (bij 0 behoudt de andere bit de waarde)
* **^ (XOR)** kan je als een **configureerbare** poort laten gedragen 
    * als een **buffer** (je configureerbare poort op **0**) 
    * als een **inverter** (je configureerbare poort op **1**)
* ^ (XOR) gebruik je in combinatie met een bitmask om te togglen (zoals een inverter maar enkel op specifieke bits)

Deze regels zullen nog veel terugkomen bij het werken met registers in MCU's!!!
