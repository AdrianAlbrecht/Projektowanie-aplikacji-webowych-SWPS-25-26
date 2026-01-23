# JavaScript – instrukcje warunkowe

## 1. Czym są instrukcje warunkowe?

Instrukcje warunkowe w JavaScript pozwalają programowi **podejmować decyzje**. Oznacza to, że w zależności od spełnienia określonego warunku:

* **jeden fragment kodu zostanie wykonany**
* **inny fragment kodu zostanie pominięty**

Bez instrukcji warunkowych program:

* zawsze wykonywałby się tak samo
* nie reagowałby na dane użytkownika
* byłby bezużyteczny w praktyce

Definicja akademicka (do egzaminu):

> **Instrukcja warunkowa** to konstrukcja językowa, która umożliwia wykonanie określonego fragmentu kodu w zależności od spełnienia warunku logicznego.

---

## 2. Warunek – co to w ogóle jest?

Warunek to **wyrażenie logiczne**, które:

* po sprawdzeniu daje wynik `true` albo `false`
* najczęściej jest wynikiem porównania

Przykład warunku:

```javascript
wiek >= 18
```

Jeśli warunek jest `true` → kod się wykona
Jeśli warunek jest `false` → kod się nie wykona

---

## 3. Instrukcja `if` – podstawowa forma warunku

Instrukcja `if` służy do wykonania kodu **tylko wtedy**, gdy warunek jest spełniony.

### Składnia

```javascript
if (warunek) {
  // kod do wykonania
}
```

### Przykład

```javascript
let wiek = 20;

if (wiek >= 18) {
  console.log("Jesteś pełnoletni");
}
```

Co tu się dzieje krok po kroku:

1. JavaScript sprawdza wartość zmiennej `wiek`
2. porównuje ją z liczbą `18`
3. jeśli warunek jest prawdziwy (`true`)
4. wykonuje kod wewnątrz bloku `{}`

---

## 4. Instrukcja `if...else`

`else` służy do wykonania **alternatywnego kodu**, gdy warunek nie zostanie spełniony.

### Składnia

```javascript
if (warunek) {
  // gdy true
} else {
  // gdy false
}
```

### Przykład

```javascript
let haslo = "1234";

if (haslo === "admin") {
  console.log("Dostęp przyznany");
} else {
  console.log("Błędne hasło");
}
```

Znaczenie:

* `if` obsługuje przypadek poprawny
* `else` obsługuje przypadek błędny

---

## 5. Instrukcja `if...else if...else`

Ta konstrukcja pozwala sprawdzić **wiele warunków po kolei**.

### Składnia

```javascript
if (warunek1) {
  ...
} else if (warunek2) {
  ...
} else {
  ...
}
```

### Przykład

```javascript
let ocena = 4;

if (ocena === 5) {
  console.log("Bardzo dobry");
} else if (ocena === 4) {
  console.log("Dobry");
} else if (ocena === 3) {
  console.log("Dostateczny");
} else {
  console.log("Niedostateczny");
}
```

JavaScript:

* sprawdza warunki od góry
* wykonuje **pierwszy spełniony**
* resztę pomija

---

## 6. Zagnieżdżanie instrukcji warunkowych

Instrukcje `if` można umieszczać **jedne w drugich**.

```javascript
let zalogowany = true;
let admin = false;

if (zalogowany) {
  if (admin) {
    console.log("Panel admina");
  } else {
    console.log("Panel użytkownika");
  }
}
```

Na egzaminie:
➡ Zagnieżdżanie jest dozwolone, ale **utrudnia czytelność**

---

## 7. Operator warunkowy (ternary operator)

Operator trójargumentowy `?:` to **skrócona wersja `if...else`**.

### Składnia

```javascript
warunek ? wartośćJeśliTrue : wartośćJeśliFalse;
```

### Przykład

```javascript
let wiek = 17;

let status = (wiek >= 18) ? "Dorosły" : "Nieletni";
```

Znaczenie:

* jeśli warunek jest spełniony → pierwsza wartość
* jeśli nie → druga wartość

Na egzaminie:
➡ używany do prostych decyzji

---

## 8. Instrukcja `switch`

`switch` służy do sprawdzania **jednej zmiennej** względem **wielu możliwych wartości**.

### Składnia

```javascript
switch (wartość) {
  case x:
    ...
    break;
  case y:
    ...
    break;
  default:
    ...
}
```

### Przykład

```javascript
let dzien = 1;

switch (dzien) {
  case 1:
    console.log("Poniedziałek");
    break;
  case 2:
    console.log("Wtorek");
    break;
  default:
    console.log("Inny dzień");
}
```

### `break` – BARDZO WAŻNE

`break`:

* przerywa działanie `switch`
* bez niego kod „przelatuje” dalej

Brak `break` = **częsty błąd egzaminacyjny**

---

## 9. Warunki logiczne w `if`

Warunki można łączyć operatorami logicznymi:

```javascript
if (wiek >= 18 && wiek < 65) {
  console.log("Wiek produkcyjny");
}
```

Operatory:

* `&&` – AND (i)
* `||` – OR (lub)
* `!` – NOT (negacja)

---

## 10. Typowe błędy

* `=` zamiast `===`
* brak nawiasów `{}`
* brak `break` w `switch`
* zbyt skomplikowane zagnieżdżenia
