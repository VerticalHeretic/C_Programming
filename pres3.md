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

---

## Wytłumaczenie alokacji

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
