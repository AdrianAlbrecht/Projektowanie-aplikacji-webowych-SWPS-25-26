# CSS – kaskadowe arkusze stylów 

## 1. Czym jest CSS?

**CSS (Cascading Style Sheets)** to język służący do opisywania **wyglądu** elementów HTML. Podczas gdy HTML odpowiada za **strukturę i znaczenie treści**, CSS decyduje o tym:

* jak strona wygląda,
* jakie ma kolory,
* jak rozmieszczone są elementy,
* jak prezentuje się tekst, tabele i obrazy.

Najważniejsza zasada:

> **HTML mówi CO jest na stronie, CSS mówi JAK to wygląda**.

Bez CSS strona działa, ale wygląda surowo i nieczytelnie.

---

## 2. Sposoby użycia (wstawiania) CSS do HTML

Istnieją **trzy podstawowe sposoby** dodania stylów CSS do dokumentu HTML.

---

### 2.1 Styl liniowy (inline)

Styl zapisany bezpośrednio w znaczniku HTML.

```html
<p style="color: red; font-size: 20px;">Tekst</p>
```

Cechy:

* działa tylko na jeden element,
* trudny w utrzymaniu,
* **łamie ideę oddzielenia struktury od wyglądu**.

➡️ Używany sporadycznie (np. testy, szybkie poprawki).

---

### 2.2 Styl wewnętrzny (w znaczniku `<style>`)

Styl zapisany w sekcji `<head>` dokumentu HTML.

```html
<style>
  p {
    color: blue;
  }
</style>
```

Cechy:

* działa na całą stronę,
* lepszy niż inline,
* nadal mało skalowalny.

---

### 2.3 Zewnętrzny arkusz stylów (REKOMENDOWANY)

Styl zapisany w osobnym pliku `.css`.

```html
<link rel="stylesheet" href="style.css">
```

Cechy:

* najlepsza praktyka,
* łatwa modyfikacja,
* jeden plik CSS może obsługiwać wiele stron.

---

## 3. Wydzielanie bloków na stronie

HTML używa elementów **blokowych**, które naturalnie tworzą sekcje strony.

Najczęściej używane znaczniki blokowe:

```html
<div>Blok</div>
<section>Sekcja</section>
<article>Artykuł</article>
```

CSS pozwala:

* ustalać szerokość i wysokość bloków,
* nadawać im kolory,
* rozmieszczać je na stronie.

Przykład:

```css
div {
  width: 300px;
  padding: 10px;
  border: 1px solid black;
}
```

---

## 4. Kaskadowość stylów (NAJWAŻNIEJSZE!)

Słowo **Cascading** oznacza, że style:

* mogą się nadpisywać,
* mają różne priorytety,
* są liczone według konkretnych zasad.

Kolejność ważności:

1. Styl inline
2. ID (`#id`)
3. Klasa (`.class`)
4. Znacznik (`p`, `div`)
5. Kolejność w pliku

Przykład konfliktu:

```css
p { color: blue; }
.text { color: green; }
#info { color: red; }
```

➡️ Element z `id="info"` będzie czerwony.

---

## 5. Budowanie stylów – selektory CSS

### 5.1 Selektor znacznika

```css
p {
  font-size: 16px;
}
```

Działa na **wszystkie znaczniki danego typu**.

---

### 5.2 Selektor ID

```css
#header {
  background-color: gray;
}
```

Cechy:

* ID jest unikalne,
* bardzo wysoki priorytet.

---

### 5.3 Selektor klasy

```css
.box {
  border: 2px solid black;
}
```

Cechy:

* może występować wiele razy,
* najczęściej używany selektor.

---

### 5.4 Selektory strukturalne

Selektory zależne od położenia elementu.

```css
p:first-child { color: red; }
p:last-child { color: blue; }
p:nth-child(2) { color: green; }
```

Używane do stylowania elementów **bez dodatkowych klas**.

---

## 6. Właściwości CSS do stylowania tekstu

Najczęściej używane:

```css
p {
  color: black;
  font-size: 18px;
  font-family: Arial;
  font-weight: bold;
  text-align: justify;
  line-height: 1.5;
}
```

Znaczenie:

* `color` – kolor tekstu
* `font-size` – rozmiar czcionki
* `font-family` – krój pisma
* `font-weight` – grubość
* `text-align` – wyrównanie
* `line-height` – wysokość linii

---

## 7. Zmiana tła elementów

```css
body {
  background-color: #f0f0f0;
}
```

Możliwe właściwości:

* `background-color`
* `background-image`
* `background-repeat`
* `background-position`

---

## 8. Stylowanie tabel

Podstawowe właściwości CSS dla tabel:

```css
table {
  border-collapse: collapse;
  width: 100%;
}

th, td {
  border: 1px solid black;
  padding: 8px;
  text-align: center;
}

tr:nth-child(even) {
  background-color: #eeeeee;
}
```

Efekty:

* scalone obramowania,
* czytelne dane,
* naprzemienne kolory wierszy.

---

## 9. Najważniejsze rzeczy na egzamin

* CSS odpowiada za wygląd
* Najlepszy jest zewnętrzny plik `.css`
* Kaskadowość = priorytety stylów
* Klasy > znaczniki
* ID tylko jeden raz
* Tekst, tło i tabele stylujemy CSS

---

**Zapamiętaj**: CSS nie zmienia treści – zmienia jej prezentację.
