---
author: Łukasz Stachnik
date: dd-MM-YYYY
---

# Hello C!

Język C, kilka najważnieszych faktów

- jest niebezpieczny
- easy to learn, hard to master
- stworzony dużo przed tym jak na chleb mówiliśmy pep, w 1972
- proceduralny, imperatywny

---

# Witaj świecie!

```c
#include <stdio.h> // Importujemy bibliotekę `standard in-out`

int main() { // Używamy funckji main, którą musi mieć każdy program w C
  printf("Hello, World!\n"); // Funkcja printf wypisuje sformatowany ciągi znaków
  return 0; // Kończymy program i zwracamy sukces
}
```

---

## Importy, headery, biblioteki, lasery

Praktycznie każdy program w C, będzie używać przynajmniej jednej z bibliotek: `stdio` albo `stdlib`.

### Jak działają biblioteki w C?

Biblioteki w C, takie jak stdio i stdlib, są zazwyczaj dostarczane jako część standardowej dystrybucji kompilatora C
(np. GCC). Mogą być zaimplementowane jako biblioteki statyczne (.a) lub dynamiczne (.so, .dll), które są ładowane w
trakcie wykonania programu lub dołączane do programu w trakcie linkowania.

### Importowanie za pomocą headerów

Biblioteki importujemy po przez pliki nagłówkowe, header files.

```c
#include <stdio.h>
```

### Jak działają headery?

https://www.youtube.com/watch?v=tOQZlD-0Scc&t=122s

---

## Exit status

W C kod, który ma się wykonać musi być umieszczony w funkcji `main` która jest punktem “_startowym_”. Jest to funkcja
która zwraca liczbę całkowitą zależnie od statusu, z jakim zakończy się program na przykład:

- **0**, jeśli program zakończy się sukcesem. Reprezentowane przez makro z biblioteki `stdlib.h` jako `EXIT_SUCCESS`
- **1**, jeśli program zakończył się niepowodzeniem. Reprezentowane przez makro z biblioteki `stdlib.h` jako
  `EXIT_FAILURE`
- **Inne wartości specyficzne dla implementacji**: Oprócz **`0`** i **`EXIT_FAILURE`**, program może zwrócić inne
  wartości całkowite. Te wartości mogą być używane do przekazywania bardziej szczegółowych informacji o wyniku działania
  programu lub o napotkanych błędach. Konwencje dotyczące tych wartości różnią się w zależności od systemu i zazwyczaj
  są dokumentowane przez producentów kompilatorów lub systemów operacyjnych.

---

## Co jest fajego w printf?

```c
#include <stdio.h>

int main() {
    printf("Hello, World!\n");
    printf("Liczba całkowita: %d\n", 10);
    printf("Liczba zmiennoprzecinkowa: %.2f\n", 3.14159);
    printf("Ciąg znaków: %s\n", "przykład");
    return 0;
}
```

---

# Dlaczego programy w C musimy budować, i jak to zrobić?

## Dlaczego?

1. **Preprocesowanie**: Przetwarza dyrektywy preprocesora, takie jak **`#include`** i **`#define`**, rozszerzając kod
   źródłowy przed właściwą kompilacją.
2. **Kompilacja**: Przekształca kod źródłowy C na kod assemblera dla docelowej platformy.
3. **Asemblacja**: Konwertuje kod assemblera na kod maszynowy, tworząc pliki obiektowe.
4. **Linkowanie**: Łączy wszystkie pliki obiektowe i biblioteki w jeden plik wykonywalny, rozwiązując odwołania do
   funkcji i zmiennych.

---

## Jak?

### Unix/Linux

#### GCC

```shell
gcc hello_world.c -o hello_world
```

#### Clang

```shell
clang hello_world.c -o hello_world
```

#### Uruchomienie

```shell
./hello_world
```

### Windows

```shell
gcc hello_world.c -o hello_world.exe
```

A następnie:

```shell
.\hello_world.exe
```

---

# Zmienne i ich typy

## Liczby całkowite

1. `char`
2. `int`
3. `short`
4. `long`
5. `long long`

### Liczby całkowite dodatnie

1. `unsigned char`
2. `unsigned int`
3. `unsigned short`
4. `unsigned long`
5. `unsigned long long`

## Liczby zmiennoprzecinkowe

1. `float`
2. `double`

### Liczby zmiennoprzecinkowe dodatnie

1. `unsigned foat`
2. `unsigned double`

## Struktury

O tym będzie później w [[pres2|drugiej prezentacji]].

---

## Gdzie jest `bool`?

```c
#define BOOL char
#define FALSE 0
#define TRUE 1
```

## Gdzie jest `string`?

C nie jest językiem obiektowym także `string` jako obiekt nie istnieje. Natomiast w C definiuje się go jako tablic
znaków (`char`). Poruszymy to w [[pres1#Ciągi znaków - Stringi]]

---

# Deklaracja zmiennych

```c
int foo;
int bar = 1;

int j = 2, p = 1, r = 3, m = 7;

const int liczba = 17;

char vat = 'V';
char vat = 'Vat' // To spowoduje warning w kompilatorze, że dany typ jest za mały dla tej zmiennej. I że zostanie zastosowana konwersja stratna.
```

---

# Tablice

```c
/* definiuje tablicę 10 liczb całkowitych */
int numbers[10];

/* wypełnienie tablicy */
numbers[0] = 10;
numbers[1] = 20
numbers[2] = 30
numbers[3] = 40
numbers[4] = 50
numbers[5] = 60;
numbers[6] = 70;

/* wypisanie siódmej liczby z tablicy, która ma indeks 6 */
printf("Siódma liczba w tablicy to %d", numbers[6]);
```

---

# Tablice wielowymiarowe

```c
int foo[1][2][3];

// Albo 

char vowels[1][5] = { 
	{'a','e','i','o','u'}
};
```

---

# Warunki logiczne

## `if`

```c
int foo = 1;
int bar = 2;

if (foo < bar) {
    printf("foo is smaller than bar.");
} else {
    printf("foo is greater than bar.");
}
```

---

## `else`

```c
int foo = 1;
int bar = 2;

if (foo < bar) {
    printf("foo is smaller than bar.");
} else {
    printf("foo is greater than bar.");
}
```

---

## `else if`

````c
int foo = 1;
int bar = 2;

if (foo < bar) {
    printf("foo is smaller than bar.");
} else if (foo == bar) {
    printf("foo is equal to bar.");
} else {
    printf("foo is greater than bar.");
}
```

---
## `switch`

Instrukcja swiftch powinna być używana w sytuacjach gdy warunków jest więcej niż dwa lub zawsze chcemy wykonać jakaś
akcję niezależnie od warunku. Dodatkowo switch jest szybsza opcją niż if jeśli chodzi o wydajność aplikacji w C.

https://www.youtube.com/watch?v=fjUG_y5ZaL4

```c
int day = 4;

switch (day) {
  case 6:
    printf("Today is Saturday");
    break;
  case 7:
    printf("Today is Sunday");
    break;
  default:
    printf("Looking forward to the Weekend");
}
````

---

# Ciągi znaków - Stringi

Ciągi znaków w języku C są w rzeczywistości tablicami znaków. Chociaż używanie wskaźników w C jest zaawansowanym
tematem, który poruszymy w następnych prezentacjach, to użyjemy wskaźników do tablicy znaków, aby zdefiniować proste
ciągi w następujący sposób:

```c
char * name = "Jan Paweł";
```

Metoda ta tworzy ciąg znaków, którego możemy używać tylko do odczytu. Jeśli chcemy zdefiniować ciąg, którym można
manipulować, będziemy musieli zdefiniować go jako lokalną tablicę znaków:

```c
char name[] = "Jan Paweł";
```

Ta notacja jest inna, ponieważ alokuje zmienną tablicową, dzięki czemu możemy nią manipulować. Notacja z pustymi
nawiasami [] mówi kompilatorowi, aby automatycznie obliczył rozmiar tablicy. W rzeczywistości jest to to samo, co jawne
przydzielenie jej, dodając jeden do długości łańcucha:

```c
char name[] = "Jan Paweł";
/* jest takie samo jak */
char name[10] = "Jan Paweł";
```

Powodem, dla którego musimy dodać jeden, chociaż ciąg `Jan Paweł` ma dokładnie 9 znaków, jest zakończenie ciągu: znak
specjalny (równy 0), który wskazuje koniec ciągu. Koniec łańcucha jest oznaczony, ponieważ program nie zna długości
łańcucha - tylko kompilator zna go zgodnie z kodem.

---

## Długość ciągów znaków

```c
char * name = "Luke Skywalker";
printf("%d\n",strlen(name));
```

---

## Porównywanie ciągów znaków

Funkcja `strncmp` porównuje dwa ciągi znaków, zwracając liczbę 0, jeśli są równe, lub inną liczbę, jeśli są różne.
Argumentami są dwa porównywane ciągi znaków i maksymalna długość porównania. Istnieje również niebezpieczna wersja tej
funkcji o nazwie `strcmp`, ale nie zaleca się jej używania. Na przykład:

```c
char * name = "John";

if (strncmp(name, "John", 4) == 0) {
    printf("Hello, John!\n");
} else {
    printf("You are not John. Go away.\n");
}
```

---

## Łączenie ciągów znaków

Funkcja `strncat` dołącza pierwsze `n` znaków łańcucha źródłowego do łańcucha docelowego, gdzie `n` to
`min(n,length(src))`; Przekazywane argumenty to łańcuch docelowy, łańcuch źródłowy i n - maksymalna liczba znaków do
dołączenia. Na przykład:

```c
char dest[20]="Hello";
char src[20]="World";
strncat(dest,src,3); // HelloWor
printf("%s\n",dest);
strncat(dest,src,20); // HelloWorWorld ale jak nie wykonamy pierwszej konkatynacji to HelloWorld
printf("%s\n",dest);
```

---

# Pętle

## `for`

```c
int array[10] = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
int sum = 0;
int i;

for (i = 0; i < 10; i++) {
    sum += array[i];
}

/* sum now contains a[0] + a[1] + ... + a[9] */
printf("Sum of the array is %d\n", sum);
```

---

## `while`

```c
int n = 0;
while (n < 10) {
    n++;
}
```

---

## Dyrektywy pętli

### `break`

```c
int n = 0;
while (1) {
    n++;
    if (n == 10) {
        break;
    }
}
```

### `continue`

```c
int n = 0;
while (n < 10) {
    n++;

    /* check that n is odd */
    if (n % 2 == 1) {
        /* go back to the start of the while block */
        continue;
    }

    /* we reach this code only if n is even */
    printf("The number %d is even.\n", n);
}
```

---

# Funkcje

```c
int foo(int bar) {
    /* do something */
    return bar * 2;
}

void car(int bar) {
    printf(bar);
}

int main() {
    foo(1);
}
```
