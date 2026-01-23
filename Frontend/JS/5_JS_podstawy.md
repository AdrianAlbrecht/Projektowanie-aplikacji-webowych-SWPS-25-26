# JavaScript — czym jest i do czego służy? Podstawy składni i typy danych
## Czym właściwie jest JavaScript?

JavaScript jest **językiem programowania**, który powstał po to, aby strony internetowe przestały być tylko statycznymi dokumentami, a zaczęły **reagować na użytkownika**. W przeciwieństwie do HTML i CSS, które opisują odpowiednio strukturę i wygląd strony, JavaScript odpowiada za **logikę i zachowanie**.

Dzięki JavaScript strona może reagować na kliknięcia, zmieniać treść bez przeładowania, sprawdzać poprawność formularzy, komunikować się z serwerem oraz dynamicznie generować elementy. Jest to więc język, który sprawia, że strona „żyje”.

Co istotne, JavaScript **działa w przeglądarce użytkownika**, a nie na serwerze (choć istnieje także JavaScript po stronie serwera, np. Node.js — ale to temat późniejszy). Oznacza to, że kod JS jest wykonywany lokalnie na komputerze osoby odwiedzającej stronę.

---

## JavaScript a Java — częsty błąd

Pomimo podobnej nazwy, JavaScript **nie jest Javą**. Są to dwa zupełnie różne języki programowania, o innej składni, innym przeznaczeniu i innym modelu działania. Jedynym powodem podobnej nazwy były względy marketingowe.

Na egzaminie warto jasno powiedzieć:
**JavaScript ≠ Java**.

---

## Gdzie piszemy JavaScript?

Kod JavaScript może być umieszczony bezpośrednio w pliku HTML albo w osobnym pliku `.js`. Najlepszą praktyką jest trzymanie kodu JS w osobnym pliku, ponieważ poprawia to czytelność i organizację projektu.

JavaScript jest interpretowany przez przeglądarkę linijka po linijce, od góry do dołu. Z tego powodu **kolejność kodu ma znaczenie**, szczególnie gdy pracujemy z DOM.

---

## Instrukcje i średniki

Kod JavaScript składa się z instrukcji, czyli poleceń, które są wykonywane przez interpreter. Instrukcje zazwyczaj kończy się średnikiem, choć JavaScript potrafi je w wielu przypadkach dopowiedzieć automatycznie. Mimo to **dobrą praktyką jest zawsze stawiać średniki**, ponieważ zmniejsza to ryzyko błędów.

Przykład prostej instrukcji:

```javascript
console.log("Hello world");
```

Powyższy kod wyświetla tekst w konsoli przeglądarki i jest często używany do debugowania programu.

---

## Zmienne — przechowywanie danych

Zmienne służą do przechowywania danych w pamięci programu. W JavaScript do deklarowania zmiennych używa się słów kluczowych `let`, `const` oraz (historycznie) `var`.

### `let` — zmienna, którą można zmieniać

```javascript
let liczba = 10;
liczba = 20; // poprawne
```

Zmienne zadeklarowane przy pomocy `let` mogą zmieniać swoją wartość w trakcie działania programu. Najczęściej używa się ich wtedy, gdy dana wartość ma się zmieniać, np. licznik kliknięć.

---

### `const` — stała

```javascript
const pi = 3.14;
// pi = 4; // błąd
```

`const` służy do deklarowania wartości, które **nie powinny się zmieniać**. Próba przypisania nowej wartości do takiej zmiennej spowoduje błąd.

Na egzaminie ważne:
➡ `const` nie oznacza „niezmienny obiekt”, tylko „niezmienny adres w pamięci”.

---

### `var` — przestarzałe podejście

```javascript
var x = 5;
```

`var` było używane w starszych wersjach JavaScript, ale obecnie **nie jest zalecane**, ponieważ ma problematyczny zasięg (scope). Na egzaminie wystarczy wiedzieć, że istnieje, ale **nie powinno się go używać w nowym kodzie**.

---

## Typy danych w JavaScript

JavaScript jest językiem **dynamicznie typowanym**, co oznacza, że nie trzeba określać typu zmiennej podczas jej deklaracji. Typ jest nadawany automatycznie na podstawie przypisanej wartości.

---

### Typ `number`

Typ `number` służy do przechowywania liczb — zarówno całkowitych, jak i zmiennoprzecinkowych.

```javascript
let a = 10;
let b = 3.5;
```

JavaScript **nie rozróżnia** liczb całkowitych i rzeczywistych — wszystkie są typu `number`.

---

### Typ `string`

Typ `string` służy do przechowywania tekstu. Tekst może być zapisany w cudzysłowach pojedynczych, podwójnych lub w tzw. backtickach.

```javascript
let imie = "Jan";
let nazwisko = 'Kowalski';
let pelneImie = `Jan Kowalski`;
```

Backticki pozwalają na wstawianie zmiennych do tekstu:

```javascript
let wiek = 20;
let tekst = `Mam ${wiek} lat`;
```

---

### Typ `boolean`

Typ logiczny przechowuje jedną z dwóch wartości: `true` albo `false`.

```javascript
let czyPelnoletni = true;
let czyZalogowany = false;
```

Booleany są bardzo często używane w instrukcjach warunkowych.

---

### Typ `undefined`

Zmienne, którym **nie przypisano wartości**, mają wartość `undefined`.

```javascript
let x;
console.log(x); // undefined
```

Oznacza to, że zmienna istnieje, ale nie ma jeszcze żadnej wartości.

---

### Typ `null`

`null` oznacza **celowy brak wartości**.

```javascript
let user = null;
```

Na egzaminie warto pamiętać różnicę:

* `undefined` — brak przypisania
* `null` — świadomie ustawiony brak danych

---

### Typ `object`

Obiekty służą do przechowywania bardziej złożonych danych w postaci par klucz–wartość.

```javascript
let osoba = {
  imie: "Anna",
  wiek: 25,
  student: true
};
```

Obiekty są podstawą działania JavaScript i DOM — większość rzeczy w JS to obiekty.

---

### Typ `array` (tablica)

Tablica to specjalny rodzaj obiektu, który przechowuje wiele wartości w określonej kolejności.

```javascript
let liczby = [1, 2, 3, 4];
```

Elementy tablicy są indeksowane od zera:

```javascript
console.log(liczby[0]); // 1
```

---

## Operator `typeof`

Aby sprawdzić typ danych zmiennej, używa się operatora `typeof`.

```javascript
typeof 10;        // "number"
typeof "tekst";   // "string"
typeof true;      // "boolean"
typeof {};        // "object"
typeof [];        // "object" (ważne!)
```

Na egzaminach często pojawia się pułapka:
➡ tablica ma typ `"object"`.

## Blok kodu w JavaScript – co to w ogóle jest?

Blok kodu w JavaScript to **zbiór instrukcji**, które są traktowane jako **jedna całość logiczna**. Blok kodu jest zawsze ograniczony **klamrami `{}`**. Przeglądarka wie wtedy, że wszystkie instrukcje znajdujące się wewnątrz klamer należą do jednego fragmentu programu.

Bloki kodu są wykorzystywane m.in. w:

* instrukcjach warunkowych (`if`)
* pętlach (`for`, `while`)
* funkcjach
* zasięgu zmiennych (`let`, `const`)

Przykład prostego bloku kodu:

```javascript
{
  let x = 10;
  console.log(x);
}
```

Ten blok zawiera dwie instrukcje:

1. Utworzenie zmiennej `x`
2. Wypisanie jej wartości w konsoli

Blok **nie wykonuje się sam z siebie** — musi być częścią jakiejś konstrukcji (np. `if`, pętli albo funkcji).

---

### Dlaczego bloki są ważne?

Bloki kodu **ograniczają zasięg zmiennych**, czyli określają, gdzie dana zmienna istnieje i gdzie można jej używać. Zmienna zadeklarowana wewnątrz bloku **nie jest widoczna poza nim** (dotyczy `let` i `const`).

To jest bardzo ważne na egzaminie.

Przykład:

```javascript
{
  let a = 5;
}

console.log(a); // błąd
```

Tutaj występuje błąd, ponieważ zmienna `a` istnieje **tylko wewnątrz bloku**, a poza nim już nie.

---

## Zmienne – po co są i jak działają?

Zmienne w JavaScript służą do **przechowywania danych w pamięci programu**. Można w nich zapisać liczby, tekst, wartości logiczne, obiekty i wiele innych danych.

JavaScript jest językiem **dynamicznie typowanym**, co oznacza, że:

* nie musimy podawać typu zmiennej
* typ jest nadawany automatycznie na podstawie przypisanej wartości

---

### `let` – zmienna, którą można zmieniać

`let` służy do deklarowania zmiennych, których wartość **może się zmieniać w trakcie działania programu**.

```javascript
let licznik = 0;
licznik = licznik + 1;
```

W tym przykładzie:

* tworzymy zmienną `licznik`
* zapisujemy w niej wartość `0`
* następnie zmieniamy jej wartość na `1`

`let` ma **zasięg blokowy**, czyli działa tylko w bloku `{}`.

---

### `const` – stała (wartość niezmienna)

`const` służy do deklarowania zmiennych, których **nie wolno zmieniać po przypisaniu**.

```javascript
const PI = 3.14;
```

Jeśli spróbujemy zmienić wartość:

```javascript
PI = 4; // błąd
```

Przeglądarka zgłosi błąd, ponieważ `const` oznacza stałe przypisanie.

Na egzaminie ważne zdanie:

> `const` blokuje możliwość zmiany przypisania, a nie zawartości obiektu.

---

### `var` – zmienna przestarzała

`var` to stary sposób deklarowania zmiennych. Obecnie **nie powinno się go używać**, ponieważ:

* nie ma zasięgu blokowego
* może powodować trudne do znalezienia błędy

```javascript
var x = 10;
```

Na egzaminie:
➡ **wystarczy wiedzieć, że istnieje i dlaczego się go nie zaleca**.

---

## Operatory – jak JavaScript wykonuje operacje?

Operatory to symbole, które pozwalają wykonywać operacje na danych, np. dodawanie, porównywanie czy przypisywanie wartości.

---

### Operatory przypisania

Najczęściej używany operator to znak `=`.

```javascript
let x = 5;
```

Tutaj:

* prawa strona (`5`) jest przypisywana
* do zmiennej po lewej stronie (`x`)

Skrócone operatory:

```javascript
x += 2; // x = x + 2
x -= 1; // x = x - 1
```

---

### Operatory arytmetyczne

Służą do operacji matematycznych.

```javascript
let a = 10;
let b = 3;

a + b; // dodawanie
a - b; // odejmowanie
a * b; // mnożenie
a / b; // dzielenie
a % b; // reszta z dzielenia
```

Operator `%` (modulo) często pojawia się na egzaminach.

---

### Operatory porównania

Służą do porównywania wartości i **zwracają wartość logiczną (`true` lub `false`)**.

```javascript
5 == "5";   // true
5 === "5";  // false
```

BARDZO WAŻNE NA EGZAMINIE:

* `==` porównuje **wartości**
* `===` porównuje **wartości i typy**

Zalecane jest używanie `===`.

---

### Operatory logiczne

Służą do pracy na wartościach logicznych.

```javascript
true && false; // AND
true || false; // OR
!true;         // NOT
```

Są często używane w instrukcjach warunkowych.

---

## Jak umieścić kod JavaScript w HTML?

Kod JavaScript można dodać do strony HTML **na dwa podstawowe sposoby**:

1. wewnątrz znacznika `<script>`
2. w osobnym pliku `.js`

---

### JavaScript w znaczniku `<script>`

Kod JS można umieścić bezpośrednio w pliku HTML za pomocą znacznika `<script>`.

```html
<!DOCTYPE html>
<html>
<head>
  <title>Przykład</title>
</head>
<body>

  <script>
    console.log("Hello world");
  </script>

</body>
</html>
```

Ten kod zostanie wykonany w momencie, gdy przeglądarka dotrze do znacznika `<script>`.

Wadą tego rozwiązania jest:

* gorsza czytelność
* mieszanie HTML z JS

---

### JavaScript w osobnym pliku (zalecane)

Najlepszą praktyką jest umieszczanie kodu JS w osobnym pliku, np. `script.js`.

**Plik HTML:**

```html
<script src="script.js"></script>
```

**Plik `script.js`:**

```javascript
console.log("Kod z osobnego pliku");
```

Zalety:

* lepsza organizacja kodu
* możliwość ponownego użycia
* łatwiejsze debugowanie

Na egzaminie:
➡ **to rozwiązanie jest uznawane za poprawne i profesjonalne**

---

### Atrybut `defer`

Atrybut `defer` powoduje, że kod JS wykona się **dopiero po załadowaniu HTML**.

```html
<script src="script.js" defer></script>
```

To zapobiega problemom z dostępem do elementów DOM.
