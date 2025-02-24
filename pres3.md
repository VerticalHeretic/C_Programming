---
author: Łukasz Stachnik
date: dd-MM-YYYY
---

# Dynamiczna alokacja pamięci

Dynamiczna alokacja pamięci jest bardzo ważnym zagadnieniem w C. Umożliwia ona budowanie złożonych struktur danych,
takich jak listy połączone. Dynamiczna alokacja pamięci pomaga nam przechowywać dane bez początkowej znajomości ich
rozmiaru w czasie pisania programu.

---

## Alokacja

Załóżmy, że chcemy dynamicznie zaalokować strukturę person. Osoba jest zdefiniowana w następujący sposób:

```c
typedef struct {
    char * name;
    int age;
} Person;
```

Aby przydzielić nową osobę w argumencie myperson, używamy następującej składni:

```c
Person * myperson = (Person *) malloc(sizeof(Person));
```

Mówi to kompilatorowi, że chcemy dynamicznie przydzielić tylko tyle, aby pomieścić strukturę person w pamięci, a
następnie zwrócić wskaźnik typu person do nowo przydzielonych danych. Funkcja alokacji pamięci malloc() rezerwuje
określoną przestrzeń pamięci.

### Wytłumaczenie alokacji

Powodem, dla którego piszemy (person *) przed wywołaniem malloc() jest to, że malloc() zwraca "void pointer", czyli
wskaźnik, który nie ma typu. Napisanie (person *) przed nim nazywa się rzutowaniem typu i zmienia typ wskaźnika
zwróconego z malloc() na person. Nie jest jednak konieczne pisanie tego w ten sposób, ponieważ C niejawnie
przekonwertuje typ zwróconego wskaźnika na typ wskaźnika, do którego jest przypisany (w tym przypadku myperson), jeśli
go nie rzutujesz.

Należy zauważyć, że sizeof nie jest rzeczywistą funkcją, ponieważ kompilator interpretuje ją i tłumaczy na rzeczywisty
rozmiar pamięci struktury person.

---

## Dostęp

Aby uzyskać dostęp do członków struktury person, możemy użyć notacji ->:

```c
myperson->name = "John";
myperson->age = 27;
```

---

Po zakończeniu korzystania z dynamicznie alokowanej struktury możemy ją zwolnić za pomocą funkcji `free`:

```c
free(myperson);
```

---

# Tablice i wskaźniki

W powszedniej presentacji [[pres2#Wskaźniki -> Pointers]] dowiedzieliśmy się, że wskaźnik do danego typu danych może
przechowywać adres dowolnej zmiennej.

```c
char c = 'A';
char *pc = &c;
```

W tym przypadku akurat c jest zmienna skalarną, tłumacząc na ludzkie ma możliwość przechowywania jednej zmiennej.
Natomiast dzisiaj zobaczymy jak zachowują się tablice i pointery.

---

```c
char vowels[] = {'A', 'E', 'I', 'O', 'U'}; // samogłoski :) 
char *pvowels = vowels; 
int i;

// Print the addresses
for (i = 0; i < 5; i++) {
   printf("&vowels[%d]: %p, pvowels + %d: %p, vowels + %d: %p\n", i, &vowels[i], i, pvowels + i, i, vowels + i);
}

// Print the values
for (i = 0; i < 5; i++) {
   printf("vowels[%d]: %c, *(pvowels + %d): %c, *(vowels + %d): %c\n", i, vowels[i], i, *(pvowels + i), i, *(vowels + i));
}
```

---

W konsoli powinniśmy zobaczyć coś podobnego:

```c
&vowels[0]: 0x7ffee146da17, pvowels + 0: 0x7ffee146da17, vowels + 0: 0x7ffee146da17

&vowels[1]: 0x7ffee146da18, pvowels + 1: 0x7ffee146da18, vowels + 1: 0x7ffee146da18

&vowels[2]: 0x7ffee146da19, pvowels + 2: 0x7ffee146da19, vowels + 2: 0x7ffee146da19

&vowels[3]: 0x7ffee146da1a, pvowels + 3: 0x7ffee146da1a, vowels + 3: 0x7ffee146da1a

&vowels[4]: 0x7ffee146da1b, pvowels + 4: 0x7ffee146da1b, vowels + 4: 0x7ffee146da1b

vowels[0]: A, *(pvowels + 0): A, *(vowels + 0): A

vowels[1]: E, *(pvowels + 1): E, *(vowels + 1): E

vowels[2]: I, *(pvowels + 2): I, *(vowels + 2): I

vowels[3]: O, *(pvowels + 3): O, *(vowels + 3): O

vowels[4]: U, *(pvowels + 4): U, *(vowels + 4): U
```

Jak łatwo się domyślić, `&vowels[i]` podaje lokalizację pamięci i-tego elementu tablicy samogłosek. Co więcej, ponieważ
jest to tablica znaków, każdy element zajmuje jeden bajt, więc kolejne adresy pamięci są oddzielone jednym bajtem.
Utworzyliśmy również wskaźnik, `pvowels`, i przypisaliśmy do niego adres tablicy samogłosek. `pvowels + i` jest poprawną
operacją; chociaż ogólnie rzecz biorąc, nie zawsze może to mieć znaczenie (omówione dalej w Arytmetyka wskaźników ). W
szczególności, wynik pokazany powyżej wskazuje, że `&vowels[i]` i `pvowels` + i są równoważne. Zachęcamy do zmiany typów
danych zmiennych tablicy i wskaźnika, aby to sprawdzić.

Jeśli przyjrzysz się uważnie poprzedniemu kodowi, zauważysz, że użyliśmy również innej pozornie zaskakującej notacji:
`vowels + i`. Co więcej, `pvowels + i` i `vowels + i` zwracają to samo - adres i-tego elementu tablicy samogłosek. Z
drugiej strony, `*(pvowels + i)` i `*(vowels + i)` zwracają i-ty element tablicy `vowels`. Dlaczego tak się dzieje?

Ponieważ sama nazwa tablicy jest (stałym) wskaźnikiem do pierwszego elementu tablicy. Innymi słowy, notacje `vowels`,
`&vowels[0]` i `vowels + 0` wskazują na tę samą lokalizację.

---

# Dynamiczna alokacja pamięci dla tablic

Wiemy już, że możemy poruszać się po tablicy za pomocą wskaźników. Co więcej, wiemy również, że możemy dynamicznie
alokować (ciągłą) pamięć za pomocą wskaźników bloków. Te dwa aspekty można połączyć w celu dynamicznej alokacji pamięci
dla tablicy. Ilustruje to poniższy kod.

```c
int n = 5; // Długość tablicy
char *pvowels = (char *) malloc(n * sizeof(char)); // Alokujemy tablice char o wielkości n
int i;

pvowels[0] = 'A';
pvowels[1] = 'E';
*(pvowels + 2) = 'I';
pvowels[3] = 'O';
*(pvowels + 4) = 'U';

for (i = 0; i < n; i++) {
    printf("%c ", pvowels[i]);
}

printf("\n");

free(pvowels);
```

---

## Kiedy tego potrzebujemy?

Należy pamiętać, że podczas deklarowania tablicy liczba elementów, które będzie ona zawierać, musi być znana wcześniej.
Dlatego w niektórych scenariuszach może się zdarzyć, że przestrzeń przydzielona dla tablicy jest mniejsza niż pożądana
przestrzeń lub większa. Jednak korzystając z dynamicznej alokacji pamięci, można przydzielić tylko tyle pamięci, ile
wymaga program. Co więcej, nieużywana pamięć może zostać zwolniona, gdy tylko przestanie być potrzebna, poprzez
wywołanie funkcji `free()`. Z drugiej strony, w przypadku dynamicznej alokacji pamięci, należy odpowiedzialnie wywoływać
funkcję `free()` w stosownych przypadkach. W przeciwnym razie może dojść do wycieków pamięci.

---

## Dynamiczna alokacja dla `n` wymiarowej tablicy

```c
int nrows = 2;
int ncols = 5;
int i, j;

// Allocate memory for nrows pointers
char **pvowels = (char **) malloc(nrows * sizeof(char *));

// For each row, allocat e memory for ncols elements
pvowels[0] = (char *) malloc(ncols * sizeof(char));
pvowels[1] = (char *) malloc(ncols * sizeof(char));

pvowels[0][0] = 'A';
pvowels[0][1] = 'E';
pvowels[0][2] = 'I';
pvowels[0][3] = 'O';
pvowels[0][4] = 'U';

pvowels[1][0] = 'a';
pvowels[1][1] = 'e';
pvowels[1][2] = 'i';
pvowels[1][3] = 'o';
pvowels[1][4] = 'u';

for (i = 0; i < nrows; i++) {
    for(j = 0; j < ncols; j++) {
        printf("%c ", pvowels[i][j]);
    }

    printf("\n");
}

// Free individual rows
free(pvowels[0]);
free(pvowels[1]);

// Free the top-level pointer
free(pvowels);
```

---

## realloc

💡 Dostępna jest jeszcze jedna przydatna komenda przy alokacjach `realloc`, która pozwala na łatwiejsza realokację
pamięci.

```c
OBJ *p = calloc(0, sizeof(OBJ)); // "zero-length" placeholder
/*...*/
while (1)
{
    p = realloc(p, c * sizeof(OBJ)); // reallocations until size settles
    /* code that may change c or break out of loop */
}
```
