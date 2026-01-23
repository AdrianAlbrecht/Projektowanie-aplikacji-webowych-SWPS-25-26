# HTML i podstawy stron WWW – materiał do nauki (wersja opisowa)

## 1. Czym jest HTML?

**HTML (HyperText Markup Language)** to język znaczników służący do tworzenia struktury stron internetowych. HTML **nie jest językiem programowania**, ponieważ nie zawiera instrukcji warunkowych, pętli ani logiki – jego zadaniem jest **opisywanie treści**.

HTML odpowiada na pytania:

* *co* znajduje się na stronie,
* *jakiego typu* to jest treść (nagłówek, akapit, lista, tabela),
* *jakie elementy są ważniejsze od innych*.

Bez HTML:

* przeglądarka nie wie, gdzie jest tekst,
* nie istnieje struktura dokumentu,
* CSS i JavaScript nie mają czego modyfikować.

---

## 2. Hipertekst – co to znaczy?

**Hipertekst** to tekst zawierający **odnośniki (linki)** do innych dokumentów lub zasobów. Dzięki hipertekstowi:

* użytkownik może poruszać się po stronach,
* Internet ma charakter nieliniowy,
* strony są ze sobą połączone w sieć.

Przykład hipertekstu w HTML:

```html
<a href="https://example.com">Przejdź do strony</a>
```

Kliknięcie linku przenosi użytkownika do innego zasobu – to jest właśnie istota hipertekstu.

---

## 3. Struktury informacji w dokumentach

### 3.1 Struktura liniowa

Struktura liniowa oznacza, że treść jest czytana **od początku do końca**, bez odgałęzień.

Przykład:

* książka,
* artykuł,
* instrukcja krok po kroku.

W HTML jest to np. ciąg akapitów:

```html
<p>Krok 1</p>
<p>Krok 2</p>
<p>Krok 3</p>
```

---

### 3.2 Struktura hierarchiczna

Struktura hierarchiczna opiera się na relacji **nadrzędny → podrzędny**.

Przykłady:

* spis treści,
* menu strony,
* struktura HTML DOM.

HTML naturalnie wspiera hierarchię:

```html
<h1>Rozdział</h1>
<h2>Podrozdział</h2>
<h3>Sekcja</h3>
```

---

### 3.3 Struktura sieciowa

Struktura sieciowa polega na **dowolnych połączeniach** między elementami.

Przykład:

* strony WWW połączone linkami,
* Wikipedia.

Realizowana jest przez hiperłącza `<a>`.

---

### 3.4 Struktura hybrydowa

Struktura hybrydowa to **połączenie wszystkich powyższych**:

* hierarchia + linki + linearna treść.

Większość nowoczesnych stron WWW ma strukturę hybrydową.

---

## 4. Model klient–serwer w kontekście WWW

### Klient

* przeglądarka (Chrome, Firefox, Edge),
* wysyła żądania HTTP,
* wyświetla HTML, CSS i JS.

### Serwer

* przechowuje pliki stron,
* obsługuje żądania,
* zwraca odpowiedź (HTML, dane, obrazy).

Schemat działania:

1. Klient wysyła żądanie (request)
2. Serwer je przetwarza
3. Serwer wysyła odpowiedź (response)

---

## 5. Akapity w HTML

Akapit to podstawowa jednostka tekstu.

Znacznik:

```html
<p>To jest akapit tekstu.</p>
```

Znacznik `<p>`:

* automatycznie tworzy odstępy,
* porządkuje tekst,
* poprawia czytelność.

---

## 6. Justowanie i wyrównywanie tekstu

HTML **nie służy do stylowania**, ale historycznie istniały takie atrybuty.

### Wyrównanie tekstu (historyczne):

```html
<p align="center">Tekst</p>
```

⚠️ Obecnie robi się to przez CSS:

```css
p {
  text-align: justify;
}
```

Rodzaje wyrównania:

* left – do lewej
* right – do prawej
* center – do środka
* justify – justowanie

---

## 7. Centrowanie elementów (pojęcie)

Centrowanie może dotyczyć:

* tekstu,
* elementu blokowego,
* obrazu.

HTML określa **co**, CSS określa **jak**.

---

## 8. Tekst preformatowany

Znacznik `<pre>` służy do wyświetlania tekstu **dokładnie tak, jak został zapisany**.

```html
<pre>
Linia 1
    Linia 2
        Linia 3
</pre>
```

Zachowuje:

* spacje,
* tabulatory,
* nowe linie.

---

## 9. Znaczniki i właściwości czcionki

### Podstawowe znaczniki tekstowe:

```html
<b>Pogrubienie</b>
<strong>Ważny tekst</strong>
<i>Kursywa</i>
<em>Podkreślenie znaczenia</em>
<u>Podkreślenie</u>
<small>Mniejszy tekst</small>
```

Znaczniki semantyczne (`strong`, `em`) są **lepsze** niż wizualne (`b`, `i`).

---

## 10. Grafika na stronie WWW

Obrazy wstawiamy znacznikiem `<img>`.

```html
<img src="obraz.jpg" alt="Opis obrazu">
```

Atrybuty:

* `src` – ścieżka do pliku
* `alt` – opis alternatywny (ważny dla dostępności)

Obraz **nie ma znacznika zamykającego**.

---

## 11. Tabele w HTML

Tabela służy do prezentowania danych tabelarycznych.

Struktura:

```html
<table>
  <tr>
    <th>Imię</th>
    <th>Wiek</th>
  </tr>
  <tr>
    <td>Jan</td>
    <td>20</td>
  </tr>
</table>
```

Znaczniki:

* `<table>` – tabela
* `<tr>` – wiersz
* `<th>` – nagłówek
* `<td>` – komórka danych

---

## 12. Najważniejsze do zapamiętania (egzamin)

* HTML opisuje strukturę, nie wygląd
* Hipertekst = linki
* DOM ma strukturę hierarchiczną
* WWW działa w modelu klient–serwer
* `<p>` to akapit
* `<pre>` zachowuje formatowanie
* Obrazy przez `<img>`
* Dane tabelaryczne przez `<table>`
