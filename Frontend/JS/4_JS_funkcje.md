#  JavaScript – FUNKCJE

## 1. Czym są funkcje w JavaScript?

Funkcja w JavaScript to **wydzielony fragment kodu**, który:

* wykonuje **konkretne zadanie**
* może być **wielokrotnie wywoływany**
* może przyjmować dane wejściowe (parametry)
* może zwracać wynik

Funkcje pozwalają:

* nie powtarzać kodu
* uporządkować program
* podzielić logikę na mniejsze, zrozumiałe części

Definicja akademicka (pod egzamin):

> **Funkcja** to blok kodu, który realizuje określone zadanie i może być wywoływany wielokrotnie w trakcie działania programu.

---

### Najprostsza funkcja

```javascript
function powitanie() {
  console.log("Cześć!");
}
```

Ta funkcja:

* ma nazwę `powitanie`
* nie przyjmuje żadnych danych
* wypisuje tekst w konsoli

Wywołanie funkcji:

```javascript
powitanie();
```

⚠️ **Funkcja się nie wykona, jeśli jej nie wywołasz**.

---

## 2. Parametry funkcji – po co są?

Parametry funkcji to **zmienne**, które:

* przekazują dane do funkcji
* pozwalają jej działać na różnych wartościach

Dzięki parametrom funkcja nie jest „sztywna”, tylko **uniwersalna**.

---

### Funkcja z jednym parametrem

```javascript
function powitaj(imie) {
  console.log("Cześć " + imie +".\n");
}
```

Wywołanie:

```javascript
powitaj("Adam");
powitaj("Ola");
```

Wynik:

```text
Cześć Adam.
Cześć Ola.
```

Tutaj:

* `imie` to parametr
* `"Adam"` i `"Ola"` to argumenty
* `\n` to znak specjalny nowej lini (w sumie działa jak ENTER)

Na egzaminie:
➡ **parametr = zmienna w definicji funkcji**
➡ **argument = wartość przekazana przy wywołaniu**

---

## 3. Funkcja bezargumentowa

Funkcja bezargumentowa to taka, która:

* **nie przyjmuje żadnych parametrów**
* zawsze działa tak samo

```javascript
function pokazCzas() {
  console.log("Godzina: 12:00");
}
```

Cechy:

* brak danych wejściowych
* brak nawiasów z parametrami w definicji
* wywołanie zawsze `nazwaFunkcji()`

Takie funkcje są używane np. do:

* wyświetlania komunikatów
* inicjalizacji strony
* obsługi prostych zdarzeń

---

## 4. Funkcja z więcej niż jednym parametrem

Funkcja może przyjmować **dowolną liczbę parametrów**, oddzielonych przecinkami.

```javascript
function dodaj(a, b) {
  return a + b;
}
```

Wywołanie:

```javascript
dodaj(2, 3); // 5
```

Co tu się dzieje:

1. `a` otrzymuje wartość `2`
2. `b` otrzymuje wartość `3`
3. funkcja zwraca ich sumę

Kolejność parametrów **ma znaczenie**.

---

## 5. Funkcja z parametrami domyślnymi

Parametry domyślne pozwalają ustawić **wartość domyślną**, gdy argument nie zostanie przekazany.

```javascript
function powitaj(imie = "Nieznajomy") {
  console.log("Cześć " + imie);
}
```

Wywołania:

```javascript
powitaj("Adam");      // Cześć Adam
powitaj();            // Cześć Nieznajomy
```

Na egzaminie:
➡ Parametry domyślne **zabezpieczają funkcję przed brakiem danych**

---

## 6. Wyrażenie funkcyjne (Function Expression)

Funkcja w JavaScript może być:

* deklaracją
* **wyrażeniem przypisanym do zmiennej**

```javascript
const dodaj = function(a, b) {
  return a + b;
};
```

Różnice:

* funkcja **nie ma nazwy**
* jest przypisana do zmiennej
* nie można jej wywołać przed deklaracją

Wywołanie:

```javascript
dodaj(2, 4);
```

---

## 7. Funkcja anonimowa

Funkcja anonimowa to funkcja:

* **bez nazwy**
* używana jednorazowo
* często jako argument innej funkcji

Przykład:

```javascript
setTimeout(function() {
  console.log("Minęły 2 sekundy");
}, 2000);
```

Tutaj:

* funkcja nie ma nazwy
* jest przekazana jako argument
* wykona się po czasie

Na egzaminie:
➡ Funkcja anonimowa **nie istnieje samodzielnie**, tylko w kontekście innej funkcji

---

## 8. Funkcja strzałkowa (Arrow Function)

Funkcja strzałkowa to **krótsza składnia** zapisu funkcji.

```javascript
const dodaj = (a, b) => {
  return a + b;
};
```

Jeszcze krócej:

```javascript
const dodaj = (a, b) => a + b;
```

Cechy:

* krótszy zapis
* brak własnego `this`
* często używana w nowoczesnym JS

Jednoparametrowa:

```javascript
const podwoj = x => x * 2;
```

---

## 9. Rekurencja – co to jest?

Rekurencja to sytuacja, w której:

> **funkcja wywołuje samą siebie**

Warunki poprawnej rekurencji:

1. warunek zakończenia (STOP)
2. zmiana argumentu przy każdym wywołaniu

Przykład – silnia:

```javascript
function silnia(n) {
  if (n === 1) {
    return 1;
  }
  return n * silnia(n - 1);
}
```

Co się dzieje:

* funkcja woła samą siebie
* za każdym razem `n` jest mniejsze
* aż dojdzie do warunku stopu

---

## 10. Rekurencja vs iteracja

### Iteracja (pętla)

Można algorytm silni zapisac też w iteracji:

```javascript
function silniaIteracyjnie(n) {
  let wynik = 1;
  for (let i = 1; i <= n; i++) {
    wynik *= i;
  }
  return wynik;
}
```

### Rekurencja

```javascript
function silniaRekurencyjnie(n) {
  if (n === 1) return 1;
  return n * silniaRekurencyjnie(n - 1);
}
```

---

### Porównanie

| Rekurencja                         | Iteracja                 |
| ---------------------------------- | ------------------------ |
| czytelna logicznie                 | szybsza                  |
| krótszy kod                        | mniejsze zużycie pamięci |
| ryzyko przepełnienia stosu pamięci | bezpieczniejsza          |