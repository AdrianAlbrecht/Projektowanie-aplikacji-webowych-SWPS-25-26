# JavaScript – obiekty, klasowość i słowo kluczowe `this`

No i tutaj wypadałoby by znać paradygmat OOP - czyli `Object-Oriented Programming`, `Programowanie Obiektowe`, paradygmat programowania, który grupuje kod wokół obiektów (instancji klas), łącząc dane (atrybuty) i funkcje (metody) w jedną całość, co pozwala na modelowanie złożonych systemów w sposób zbliżony do rzeczywistości, ułatwiając zarządzanie, modyfikację i ponowne użycie kodu. No ale postaramy się to jakoś obrać w JS.

## 1. Czym są obiekty w JavaScript?

Obiekt w JavaScript to **struktura danych**, która:

* przechowuje **wiele wartości**
* opisuje **jeden byt / jedną rzecz**
* składa się z **par klucz–wartość**

Można powiedzieć, że obiekt to:

> **grupowanie danych i zachowań w jednym miejscu**

Definicja akademicka (egzamin):

> **Obiekt** to struktura danych składająca się z właściwości (properties) oraz metod (functions), opisująca stan i zachowanie danego bytu.

---

### Dlaczego obiekty są potrzebne?

Bez obiektów:

* dane są porozrzucane po zmiennych
* kod jest chaotyczny
* trudno coś rozbudować

Z obiektami:

* dane są logicznie pogrupowane
* kod jest czytelniejszy
* łatwiej zarządzać złożonymi strukturami

---

## 2. Tworzenie obiektu – zapis literałowy

Najprostszy sposób tworzenia obiektu to **literał obiektu**, czyli zapis w `{}`.

```javascript
const user = {
  imie: "Adam",
  wiek: 25,
  zalogowany: true
};
```

Co tu mamy:

* `imie`, `wiek`, `zalogowany` → **właściwości**
* każda właściwość ma nazwę i wartość
* całość opisuje jednego użytkownika

---

### Dostęp do właściwości obiektu

```javascript
user.imie;
user["wiek"];
```

Znaczenie:

* kropka (`.`) – najczęściej używana
* nawiasy `[]` – gdy nazwa jest dynamiczna lub tekstowa

---

## 3. Metody obiektu – funkcje w obiekcie

Obiekt może zawierać **funkcje**, które nazywamy **metodami**.

```javascript
const user = {
  imie: "Adam",
  powitaj: function () {
    console.log("Cześć!");
  }
};
```

Wywołanie metody:

```javascript
user.powitaj();
```

Znaczenie:

* metoda opisuje **zachowanie obiektu**
* działa w kontekście danego obiektu

---

## 4. Słowo kluczowe `this` – CO TO JEST?

`this` to **odwołanie do obiektu**, w którego kontekście aktualnie wykonywany jest kod.

Najważniejsze zdanie do zapamiętania:

> **`this` wskazuje na obiekt, który wywołał daną funkcję**

---

### `this` w metodzie obiektu

```javascript
const user = {
  imie: "Adam",
  powitaj: function () {
    console.log("Cześć " + this.imie);
  }
};
```

Co się dzieje:

* `this.imie` odnosi się do `user.imie`
* metoda korzysta z danych własnego obiektu

Bez `this`:

* obiekt nie wiedziałby, do kogo należą dane

---

### Typowy błąd

❌

```javascript
console.log(imie);
```

✔

```javascript
console.log(this.imie);
```

---

## 5. Kontekst działania `this` (BARDZO WAŻNE)

Wartość `this` **zależy od miejsca wywołania funkcji**, a nie od miejsca jej napisania.

### Przykład

```javascript
const user1 = {
  imie: "Adam",
  pokaz: function () {
    console.log(this.imie);
  }
};

const user2 = {
  imie: "Ola",
  pokaz: user1.pokaz
};

user1.pokaz(); // Adam
user2.pokaz(); // Ola
```

Ta sama funkcja, ale inne `this`.

Na egzaminie:
➡ `this` jest **dynamiczne**

---

## 6. Obiekty a funkcje konstrukcyjne (stary styl)

Zanim pojawiły się klasy, obiekty tworzono przez **funkcje konstrukcyjne**.

```javascript
function User(imie, wiek) {
  this.imie = imie;
  this.wiek = wiek;
}
```

Tworzenie obiektu:

```javascript
const u1 = new User("Adam", 25);
```

Znaczenie:

* `new` tworzy nowy obiekt
* `this` wskazuje na nowo tworzony obiekt

---

## 7. Klasowość w JavaScript – czym jest klasa?

Klasa to **szablon (wzorzec)**, na podstawie którego tworzy się obiekty.

Definicja egzaminacyjna:

> **Klasa** to konstrukcja opisująca strukturę i zachowanie obiektów danego typu.

Klasy w JS zostały wprowadzone w ES6 i są **cukrem składniowym** (syntactic sugar) nad prototypami.

---

## 8. Tworzenie klasy – słowo kluczowe `class`

```javascript
class User {
  constructor(imie, wiek) {
    this.imie = imie;
    this.wiek = wiek;
  }

  powitaj() {
    console.log("Cześć " + this.imie);
  }
}
```

Co tu mamy:

* `class User` – definicja klasy
* `constructor` – metoda wywoływana przy tworzeniu obiektu
* `this` – odniesienie do nowej instancji

---

### Tworzenie obiektu z klasy

```javascript
const u1 = new User("Adam", 25);
u1.powitaj();
```

---

## 9. `this` w klasach

W klasach:

* `this` odnosi się do **konkretnej instancji**
* każda instancja ma własne dane

```javascript
const u2 = new User("Ola", 30);
```

`this.imie`:

* w `u1` → „Adam”
* w `u2` → „Ola”

---

## 10. Klasy vs obiekty – różnica

| Obiekt              | Klasa            |
| ------------------- | ---------------- |
| konkretna instancja | szablon          |
| jeden byt           | opis wielu bytów |
| `{}`                | `class`          |

Na egzaminie:
➡ **klasa tworzy obiekty**

---

## 11. `this` a funkcje strzałkowe (UWAGA!)

Funkcje strzałkowe:

* **nie mają własnego `this`**
* przejmują `this` z otoczenia

```javascript
const obj = {
  imie: "Adam",
  pokaz: () => {
    console.log(this.imie);
  }
};
```

 To **nie zadziała**.

Dlaczego?

* `this` nie odnosi się do `obj`
* tylko do kontekstu zewnętrznego

Na egzaminie:
➡ **nie używamy arrow function jako metod obiektu**
