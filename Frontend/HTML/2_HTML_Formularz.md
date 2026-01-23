# Formularze HTML

---

Formularze HTML są jednym z podstawowych mechanizmów komunikacji pomiędzy użytkownikiem a aplikacją webową. To właśnie za ich pomocą użytkownik może wprowadzać dane, takie jak login, hasło, adres e-mail czy odpowiedzi w ankiecie, a następnie przekazywać je do dalszego przetwarzania. Bez formularzy strona internetowa pełniłaby jedynie funkcję informacyjną, nie umożliwiając interakcji ani zbierania danych.

Formularz w HTML nie przetwarza danych samodzielnie. Jego zadaniem jest **zebranie informacji**, ich **walidacja na podstawowym poziomie** oraz **przekazanie danych dalej** – do serwera lub do skryptu JavaScript.

---

## OK, ale jak to działa?

### 1. **Znacznik `<form>` – kontener formularza**

Podstawą każdego formularza jest znacznik `<form>`. To on informuje przeglądarkę, że znajdujące się wewnątrz elementy służą do wprowadzania danych przez użytkownika. Wszystkie pola formularza, przyciski oraz mechanizmy wysyłania danych muszą znajdować się wewnątrz tego znacznika.

Formularz pełni rolę **kontenera logicznego**, który grupuje dane i określa sposób ich przesyłania. Bez znacznika `<form>` pola tekstowe czy przyciski istnieją wizualnie, ale nie są traktowane jako spójna całość.

Przykład:

```html
<form>
  <input type="text">
</form>
```

Ten kod tworzy formularz z jednym polem tekstowym, ale sam w sobie **jeszcze nic nie robi**.

---

### 2. **Atrybuty `action` i `method` – gdzie i jak wysyłane są dane**

Formularz staje się użyteczny dopiero wtedy, gdy określimy, **co ma się stać z danymi po jego wysłaniu**. Służą do tego dwa kluczowe atrybuty: `action` oraz `method`.

Atrybut `action` określa **adres**, pod który zostaną wysłane dane formularza. Może to być adres serwera, endpoint API lub inna strona.

Atrybut `method` określa **sposób przesyłania danych**. Najczęściej spotykane metody to:

* `GET` – dane są dołączane do adresu URL (widoczne w pasku przeglądarki),
* `POST` – dane są przesyłane w treści zapytania i nie są widoczne w URL.

Przykład:

```html
<form action="/login" method="post">
```

W tym przypadku formularz wysyła dane do adresu `/login` metodą POST.

---

### 3. **Element `<input>` – wprowadzanie danych**

Element `<input>` jest podstawowym narzędziem do zbierania danych od użytkownika. Może przyjmować różne formy w zależności od atrybutu `type`, np. pole tekstowe, pole hasła, pole e-mail lub pole numeryczne.

Każde pole formularza powinno posiadać atrybut `name`. Jest to **klucz**, pod którym dana wartość zostanie wysłana. Jeśli pole nie ma atrybutu `name`, jego wartość **nie zostanie uwzględniona** podczas wysyłania formularza.

Przykład:

```html
<input type="text" name="username">
```

W tym przypadku użytkownik wpisuje nazwę użytkownika, która zostanie wysłana pod kluczem `username`.

---

### 4. **Różne typy pól i ich znaczenie**

HTML oferuje różne typy pól wejściowych, które pomagają w walidacji danych już na poziomie przeglądarki.

Pole typu `password` maskuje wprowadzane znaki, ale **nie szyfruje danych** – jest to wyłącznie zabieg wizualny.

```html
<input type="password" name="password">
```

Pole typu `email` wymusza poprawny format adresu e-mail, sprawdzając obecność znaku `@` i domeny.

```html
<input type="email" name="email">
```

Dzięki temu przeglądarka może wykryć podstawowe błędy jeszcze przed wysłaniem formularza.

---

### 5. **Znacznik `<label>` – opis pola formularza**

Znacznik `<label>` służy do opisywania pól formularza. Jego głównym zadaniem jest poprawa czytelności oraz dostępności formularza. Połączenie etykiety z polem formularza sprawia, że kliknięcie w tekst etykiety automatycznie aktywuje powiązane pole.

```html
<label for="login">Login</label>
<input id="login" type="text" name="login">
```

Jest to istotne zarówno z punktu widzenia użytkownika, jak i dostępności (np. dla czytników ekranu).

---

### 6. **Przycisk wysyłania formularza**

Aby formularz mógł zostać wysłany, konieczny jest przycisk typu `submit`. Jego kliknięcie powoduje uruchomienie całego mechanizmu wysyłania danych.

```html
<button type="submit">Wyślij</button>
```

Kliknięcie tego przycisku powoduje:

1. sprawdzenie walidacji HTML,
2. wysłanie danych,
3. przeładowanie strony (domyślne zachowanie).

---

### 7. **Domyślne zachowanie formularza**

Domyślnie przeglądarka po wysłaniu formularza:

* wysyła dane do `action`,
* przeładowuje stronę,
* przechodzi na nowy adres.

W nowoczesnych aplikacjach webowych takie zachowanie jest **niepożądane**, ponieważ utrudnia dynamiczne przetwarzanie danych.

---

### 8. **Formularz a JavaScript – przejęcie kontroli**

JavaScript pozwala przechwycić moment wysyłania formularza i **zablokować jego domyślne zachowanie**. Dzięki temu dane mogą zostać przetworzone bez przeładowywania strony.

```js
form.addEventListener("submit", function(event) {
  event.preventDefault();
});
```

Instrukcja `event.preventDefault()` informuje przeglądarkę, że **standardowa akcja wysłania formularza ma zostać zatrzymana**.

---

### 9. **Odczytywanie danych z formularza**

Po przechwyceniu formularza JavaScript może odczytać dane wpisane przez użytkownika. Każde pole formularza posiada właściwość `.value`, która przechowuje aktualną zawartość pola.

```js
const username = document.getElementById("username").value;
```

Dzięki temu możliwe jest dalsze przetwarzanie danych, np. walidacja lub wysyłka do API.

---

### 10. **Walidacja danych**

Walidacja polega na sprawdzeniu, czy dane wprowadzone przez użytkownika spełniają określone warunki, np. czy pola nie są puste lub czy hasło ma odpowiednią długość.

```js
if (username === "") {
  alert("Pole nie może być puste");
}
```

Walidacja po stronie JavaScript pozwala na pełną kontrolę nad danymi jeszcze przed ich wysłaniem.

---

### 11. **Najczęstsze błędy w formularzach**

* brak atrybutu `name`,
* brak etykiet `<label>`,
* brak `preventDefault()`,
* poleganie wyłącznie na walidacji HTML,
* brak informacji zwrotnej dla użytkownika.