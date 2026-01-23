## Tablice w JavaScript

Tablica (ang. **array**) w JavaScript to **struktura danych**, która pozwala przechowywać **wiele wartości w jednej zmiennej**. Zamiast tworzyć osobne zmienne dla każdej wartości, używamy jednej tablicy i odwołujemy się do jej elementów za pomocą indeksów.

Tablice są **jednym z najważniejszych tematów w JS** – praktycznie nie da się pisać sensownego kodu bez ich znajomości.

## 1. Tworzenie tablicy

### Najprostszy sposób – nawiasy kwadratowe

```javascript
let liczby = [10, 20, 30];
```

W tablicy:

* elementy są oddzielone przecinkami
* indeksy zaczynają się od **0**

```javascript
console.log(liczby[0]); // 10
console.log(liczby[1]); // 20
```

---

### Tablica może przechowywać różne typy danych

```javascript
let dane = ["Jan", 25, true];
```

JS **nie wymaga jednego typu danych** w tablicy, dlatego bardziej przypominają działanie list niż tablic - nawet jak się później okaże, mają metody podobne do list, czego tablice w innych języka programowania raczej nie mają.

---

## 2. Indeksy tablicy – jak to działa

Indeks to numer pozycji elementu w tablicy.

```javascript
let kolory = ["czerwony", "zielony", "niebieski"];
```

| Indeks | Wartość     |
| ------ | ----------- |
| 0      | "czerwony"  |
| 1      | "zielony"   |
| 2      | "niebieski" |

**Najczęstszy błąd**:

```javascript
kolory[3] // undefined
```

Dlaczego? Owszem obiekty są 3, ale indeksujemy od 0, a nie od 1.

---

## 3. Zmiana wartości w tablicy

```javascript
kolory[1] = "żółty";
```

Nadpisujemy element o indeksie `1`.

---

## 4. Właściwość `length`

```javascript
console.log(kolory.length);
```

`length` zwraca **liczbę elementów** w tablicy.

Ostatni element:

```javascript
kolory[kolory.length - 1];
```

---

## 5. Podstawowe metody tablic (MEGA WAŻNE)

### `push()` – dodaje element na koniec

```javascript
kolory.push("czarny");
```

---

### `pop()` – usuwa ostatni element

```javascript
kolory.pop();
```

---

### `unshift()` – dodaje element na początek

```javascript
kolory.unshift("biały");
```

---

### `shift()` – usuwa pierwszy element

```javascript
kolory.shift();
```

---

## 6. Iterowanie po tablicy

### Pętla `for`

```javascript
for (let i = 0; i < kolory.length; i++) {
    console.log(kolory[i]);
}
```

Klasyczne, egzaminacyjne rozwiązanie.

---

### `for...of` (najczytelniejsze)

```javascript
for (let kolor of kolory) {
    console.log(kolor);
}
```

---

### `forEach()`

```javascript
kolory.forEach(function(kolor) {
    console.log(kolor);
});
```

Funkcja wykona się **dla każdego elementu tablicy**.

---

## 7. Metody tablic funkcyjne (często na kolokwiach)

### `map()` – tworzy nową tablicę

```javascript
let liczby = [1, 2, 3];

let podwojone = liczby.map(function(x) {
    return x * 2;
});
```

Oryginalna tablica **nie jest zmieniana**.

---

### `filter()` – filtruje elementy

```javascript
let parzyste = liczby.filter(function(x) {
    return x % 2 === 0;
});
```

---

### `reduce()` – redukuje tablicę do jednej wartości

```javascript
let suma = liczby.reduce(function(acc, x) {
    return acc + x;
}, 0);
```

`acc` to akumulator (wynik pośredni).

---

## 8. Sprawdzanie czy coś jest tablicą

```javascript
Array.isArray(liczby);
```

Zwraca `true` lub `false`.

---

## 9. Tablice a referencje (BARDZO WAŻNE)

```javascript
let a = [1, 2, 3];
let b = a;

b.push(4);

console.log(a); // [1,2,3,4]
```

`a` i `b` wskazują **to samo miejsce w pamięci**.

### Kopia tablicy

```javascript
let c = [...a];
```

---

## 10. Najczęstsze błędy

* Indeks poza zakresem
* `i <= length` zamiast `i < length`
* Modyfikowanie tablicy w `map()`
* Mylenie `for...in` z `for...of`

---

## 11. Co zapamiętać na egzamin (ściąga)

* Indeksy od **0** do **wielkość_tablicy - 1**
* `length` → liczba elementów
* `push/pop` → koniec
* `shift/unshift` → początek
* `map` → nowa tablica
* `filter` → warunek
* `reduce` → jedna wartość
* Tablice są **referencjami**


## Tablice asocjacyjne w JavaScript – teoria + praktyka (WAŻNE NA EGZAMIN)

### UWAGA NA START (to pada na egzaminach!)

**W JavaScript NIE MA tablic asocjacyjnych.**

To nie jest złośliwość – to **fakt językowy**.
To, co wiele osób nazywa *„tablicą asocjacyjną”*, w JS jest **OBIEKTEM**.

---

## 1. Skąd to zamieszanie?

W innych językach:

* PHP → tablice asocjacyjne
* Python → słowniki (`dict`)
* C++ → mapy (`map`)

W JavaScript:

* ❌ `Array` → **indeksy liczbowe**
* ✅ `Object` / `Map` → **klucz → wartość**

---

## 2. Co mamy na myśli mówiąc „tablica asocjacyjna”?

Zazwyczaj chodzi o strukturę:

```
klucz → wartość
```

Np.:

```
"imie" → "Jan"
"wiek" → 20
```

W JS **robi się to obiektem**.

---

## 3. Obiekt jako „tablica asocjacyjna”

### Tworzenie obiektu

```javascript
let student = {
    imie: "Jan",
    wiek: 20,
    aktywny: true
};
```

To jest **obiekt**, NIE tablica.

---

### Dostęp do wartości (dwa sposoby)

#### Notacja kropkowa

```javascript
student.imie;
```

#### Notacja nawiasowa (ważna!)

```javascript
student["wiek"];
```

📌 Notacja nawiasowa jest konieczna, gdy:

* klucz jest w zmiennej
* klucz ma spacje

```javascript
let klucz = "imie";
student[klucz];
```

---

## 4. Dodawanie i usuwanie danych

```javascript
student.ocena = 5;        // dodanie
delete student.aktywny;  // usunięcie
```

---

## 5. Dlaczego NIE używać Array jako asocjacyjnej?

### BŁĘDNY KOD (częsty u studentów)

```javascript
let arr = [];
arr["imie"] = "Jan";
arr["wiek"] = 20;

console.log(arr.length); // 0
```

* `length` nie działa
* pętle tablicowe nie działają
* to NIE JEST tablica

Technicznie JS pozwala, ale **to zły styl i błąd logiczny**.

---

## 6. Iterowanie po „tablicy asocjacyjnej” (czyli obiekcie)

### `for...in`

```javascript
for (let klucz in student) {
    console.log(klucz, student[klucz]);
}
```

---

### `Object.keys()`

```javascript
Object.keys(student).forEach(function(k) {
    console.log(k, student[k]);
});
```

---

### `Object.entries()` (najczytelniejsze)

```javascript
for (let [klucz, wartosc] of Object.entries(student)) {
    console.log(klucz, wartosc);
}
```

---

## 7. Obiekt vs Map (ważne porównanie)

### `Map` – nowoczesna „prawdziwa” mapa

```javascript
let mapa = new Map();

mapa.set("imie", "Jan");
mapa.set("wiek", 20);

console.log(mapa.get("imie"));
```

### Zalety `Map`

* dowolny typ klucza
* zachowana kolejność
* łatwe iterowanie
* brak konfliktu z prototypem

---

## 8. Obiekt czy Map – co na egzamin?

| Sytuacja           | Co użyć    |
| ------------------ | ---------- |
| Dane JSON          | Object     |
| Prosta struktura   | Object     |
| Dużo operacji      | Map        |
| Klucze dynamiczne  | Map        |
| Egzamin podstawowy | **Object** |

---

## 9. Najczęstsze pytania egzaminacyjne

**❓ Czy JS ma tablice asocjacyjne?**
➡️ **Nie. Używa obiektów lub Map.**

**❓ Czy Array może mieć klucze tekstowe?**
➡️ Może, ale NIE należy tego robić.

**❓ Czym różni się Object od Map?**
➡️ Klucze, kolejność, iteracja, wydajność.

---

## 10. Ściąga egzaminacyjna

* ❌ tablice asocjacyjne ≠ Array
* ✅ „tablica asocjacyjna” = Object
* `obj[key]` > `obj.key` (dynamiczne)
* `Map` = nowoczesna mapa
* `length` działa tylko na Array
