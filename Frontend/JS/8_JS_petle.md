## Pętle w JavaScript

Pętle w JavaScript służą do **wielokrotnego wykonywania tego samego fragmentu kodu**, dopóki spełniony jest określony warunek. Zamiast pisać ten sam kod 10, 100 albo 1000 razy, **używamy pętli**, które robią to za nas automatycznie.

Najczęściej pętle wykorzystuje się do:

* przeglądania tablic (array),
* wykonywania obliczeń krok po kroku,
* reagowania na dane wejściowe,
* pracy z DOM (np. wiele elementów HTML).


## 1. Pętla `for`

### Co to jest `for`?

Pętla `for` to **najczęściej używana pętla**, gdy:

* wiemy **ile razy** chcemy wykonać kod,
* iterujemy po tablicy od początku do końca.

### Składnia

```javascript
for (inicjalizacja; warunek; krok) {
    // kod wykonywany w pętli
}
```

### Co robi każdy element?

```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

* `let i = 0` → tworzymy licznik pętli
* `i < 5` → pętla działa **dopóki warunek jest true**
* `i++` → po każdej iteracji zwiększamy licznik o 1
* `console.log(i)` → kod wykonywany w każdej iteracji

Efekt:

```
0
1
2
3
4
```

---

### `for` + tablica

```javascript
let imiona = ["Ala", "Ola", "Kuba"];

for (let i = 0; i < imiona.length; i++) {
    console.log(imiona[i]);
}
```

Pętla przechodzi po wszystkich elementach tablicy.

---

## 2. Pętla `while`

### Co to jest `while`?

Pętla `while` wykonuje kod **dopóki warunek jest spełniony**.
Nie wiemy, ile razy się wykona – liczy się tylko warunek.

### Składnia

```javascript
while (warunek) {
    // kod
}
```

### Przykład

```javascript
let i = 0;

while (i < 3) {
    console.log(i);
    i++;
}
```

Bardzo ważne:

> Jeśli **nie zmienisz warunku w pętli**, powstanie **pętla nieskończona**.

---

## 3. Pętla `do...while`

### Czym się różni od `while`?

* `do...while` **zawsze wykona się przynajmniej raz**
* warunek sprawdzany jest **po wykonaniu kodu**

### Składnia

```javascript
do {
    // kod
} while (warunek);
```

### Przykład

```javascript
let i = 5;

do {
    console.log(i);
    i++;
} while (i < 3);
```

Kod wykona się **raz**, mimo że warunek od początku jest fałszywy.

---

## 4. Pętla `for...of`

### Do czego służy?

* do iterowania po **wartościach**:

  * tablic
  * stringów
  * kolekcji

### Przykład – tablica

```javascript
let liczby = [10, 20, 30];

for (let liczba of liczby) {
    console.log(liczba);
}
```

`liczba` to **konkretna wartość**, nie indeks.

---

### Przykład – string

```javascript
for (let znak of "ABC") {
    console.log(znak);
}
```

---

## 5. Pętla `for...in`

### Do czego służy?

* do iterowania po **kluczach obiektu**

### Przykład – obiekt

```javascript
let osoba = {
    imie: "Jan",
    wiek: 25
};

for (let klucz in osoba) {
    console.log(klucz, osoba[klucz]);
}
```

`klucz` to nazwa właściwości obiektu.

**Nie używaj `for...in` do tablic** – to częsty błąd egzaminacyjny.

---

## 6. Instrukcje sterujące pętlą

### `break`

Przerywa pętlę całkowicie.

```javascript
for (let i = 0; i < 10; i++) {
    if (i === 5) {
        break;
    }
    console.log(i);
}
```

---

### `continue`

Pomija **jedną iterację**, ale pętla działa dalej.

```javascript
for (let i = 0; i < 5; i++) {
    if (i === 2) {
        continue;
    }
    console.log(i);
}
```

---

## 7. Najczęstsze błędy

* Brak zmiany warunku → pętla nieskończona
* `for...in` do tablic
* Błąd w warunku (`=` zamiast `==` lub `===`)
* Wyjście poza zakres tablicy (`i <= length`)
