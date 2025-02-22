---
author: Łukasz Stachnik
date: dd-MM-YYYY
---

# Wskaźniki -> Pointers

Wskaźnik jest zasadniczo prostą zmienną całkowitą, która przechowuje adres pamięci wskazujący na wartość, zamiast
przechowywać samą wartość.

Pamięć komputera jest sekwencyjnym magazynem danych, a wskaźnik wskazuje na określoną część pamięci.

[you will never ask about pointers again after watching this video](https://www.youtube.com/watch?v=2ybLD6_2gKM)

---

# Ciągi znaków a pointery

- Jak wiemy już z [[pres1#Ciągi znaków - Stringi]]] ciągi znaków to w rzeczywistoście tablica znaków.
- Te znaki są ułożone sekwencyjnie w pamięci (obok siebie)

```c
char * name = "Łukasz Stachnik";
```

Ponieważ wiemy, że pamięć jest sekwencyjna, możemy założyć, że jeśli przejdziemy w pamięci do następnego znaku,
otrzymamy następną literę w łańcuchu, aż dotrzemy do końca łańcucha, oznaczonego terminatorem zerowym (znak o wartości
porządkowej 0, oznaczony jako \0).

---

# Dereferencja

- Dereferencja to czynność polegająca na odwoływaniu się do miejsca, na które wskazuje wskaźnik, zamiast do adresu
  pamięci.
- Używamy już dereferencji w tablicach - ale jeszcze o tym nie wiedzieliśmy. Operator nawiasów - na przykład [0] -
  uzyskuje dostęp do pierwszego elementu tablicy. A ponieważ tablice są w rzeczywistości wskaźnikami, dostęp do
  pierwszego elementu w tablicy jest tym samym, co dereferencja wskaźnika.
- Odwoływanie się do wskaźnika odbywa się za pomocą operatora gwiazdki *.

Jeśli chcemy utworzyć wskaźnik, który będzie wskazywać na inną zmienną na naszym stosie, możemy napisać następujący kod:

```c
/* define a local variable a */
int a = 1;

/* define a pointer variable, and point it to a using the & operator */
int * pA = &a;

printf("The value a is %d\n", a);
printf("The value of a is also %d\n", *pA); // Dereference the pointer to get the underlying value
```

---

## Mutacja zmiennej przez dereferencje

```c
int a = 1;
int * pA = &a;

/* let's change the variable a */
a += 1;

/* we just changed the variable again! */
*pA += 1;

/* will print out 3 */
printf("The value of a is now %d\n", a);
```

Aby sobie ułatwić zrozumienie czym jest wskaźniki i dereferencja, dobrze jest ułożyć sobie w głowie to tak:

- (&) → Adres zmiennej …
- (*) → Wartość zmiennej wskazywanej przez …

---

# Struktury

Struktury C są specjalnymi, dużymi zmiennymi, które zawierają w sobie kilka nazwanych zmiennych. Struktury są
podstawowym fundamentem obiektów i klas w języku C. Struktury są używane do

- Serializacji danych
- Przekazywania wielu argumentów do i z funkcji poprzez pojedynczy argument
- Struktury danych, takie jak połączone listy, drzewa binarne i inne.

Najbardziej podstawowym przykładem struktur są punkty, które są pojedynczą jednostką zawierającą dwie zmienne - x i y.
Zdefiniujmy punkt:

```c
struct point {
    int x;
    int y;
};
```

---

## Przykład użycia

````c
/* draws a point at 10, 5 */
struct point p;
p.x = 10;
p.y = 5;
draw(p);
```

---

## Typedefs

Typedefy pozwalają nam definiować typy o innej nazwie - co może się przydać w przypadku struktur i wskaźników. W tym przypadku chcielibyśmy pozbyć się długiej definicji struktury `point` Możemy użyć następującej składni, aby usunąć słowo kluczowe struct za każdym razem, gdy chcemy zdefiniować nowy `point`:

```c
typedef struct {
    int x;
    int y;
} point;
```

A to zaowocuje skróceniem initalizacji do :

```c
point p;
```
---


## Struktury a wskaźniki

Struktury mogą również przechowywać wskaźniki - co pozwala im przechowywać ciągi lub wskaźniki do innych struktur - co jest ich prawdziwą mocą. Na przykład, możemy zdefiniować strukturę pojazdu w następujący sposób:

```c
typedef struct {
    char * brand;
    int model;
} vehicle;
```

Ponieważ marka jest wskaźnikiem char, typ pojazdu może zawierać ciąg znaków (który w tym przypadku wskazuje markę pojazdu):

```c
vehicle mycar;
mycar.brand = "Ford";
mycar.model = 2007;
```

---

# Po co nam wskaźniki w funkcjach?


Zakładając, że rozumiesz teraz wskaźniki i funkcje, jesteś świadomy, że argumenty funkcji są przekazywane przez wartość, co oznacza, że są kopiowane do i z funkcji. Ale co jeśli przekażemy wskaźniki do wartości zamiast samych wartości? Pozwoli nam to dać funkcjom kontrolę nad zmiennymi i strukturami funkcji nadrzędnych, a nie tylko ich kopią, a tym samym bezpośrednio odczytywać i zapisywać oryginalny obiekt.

Załóżmy, że chcemy napisać funkcję zwiększającą liczbę o jeden o nazwie `addone`. To nie zadziała:

```c
void addone(int n) {
    // n jest zmienną lokalną, która istnieje tylko w zakresie funkcji
    n++; // dlatego zwiększanie jej wartości nie ma żadnego efektu
}

int n;
printf("Before: %d\n", n); // tu będzie 0
addone(n);
printf("Po: %d\n", n); // tu będzie nadal 0
```

---

Jednak to zadziała:

```c
void addone(int *n) {
    // n jest tutaj wskaźnikiem, który wskazuje na adres pamięci poza zakresem funkcji
    (*n)++; // spowoduje to efektywne zwiększenie wartości n
}

int n;
printf("Before: %d\n", n); // Tu będzie 0
addone(&n);
printf("Po: %d\n", n); // Tu będzie 1
```

---

Różnica polega na tym, że druga wersja `addone` otrzymuje wskaźnik do zmiennej n jako argument, a następnie może nią manipulować, ponieważ wie, gdzie znajduje się w pamięci.

Zauważ, że podczas wywoływania funkcji `addone` musimy przekazać referencję do zmiennej n, a nie samą zmienną - dzieje się tak, aby funkcja znała adres zmiennej, a nie tylko otrzymała kopię samej zmiennej.

---

# Wskaźniki do struktur?

Powiedzmy, że chcemy utworzyć funkcję, która przesuwa punkt do przodu w obu kierunkach x i y, zwaną move. Zamiast wysyłać dwa wskaźniki, możemy teraz wysłać tylko jeden wskaźnik do funkcji struktury punktu:

```c
void move(point * p) {
    (*p).x++;
    (*p).y++;
}
```

Jeśli jednak chcemy odwołać się do struktury i uzyskać dostęp do jednego z jej wewnętrznych elementów, mamy do tego skróconą składnię, ponieważ operacja ta jest szeroko stosowana w strukturach danych. Możemy przepisać tę funkcję przy użyciu następującej składni:

```c
void move(point * p) {
    p->x++;
    p->y++;
}
```
````
